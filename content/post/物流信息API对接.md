---
title: 物流信息API对接
date: 2017-09-06 13:30:59
---

​	项目刚好涉及一小块儿订单部分，需要一个第三方的快递物流信息接口API做一个物流轨迹的跟踪。国内的快递100、快递鸟、快递网、聚合数据，国际快递的话使用 **Trackingmore**也不错。个人综合比较考虑一下，决定使用 [快递鸟](http://www.kdniao.com/) 物流数据平台。

<!--more-->

### 注册	

不方便的一个地方就是需要手机号收取验证码进行注册， 如果企业注册的话，可能会稍微麻烦一点。	

提交申请后，按照提示填写企业、个人信息就行，会要求上传证件信息（无需等待审核）。

注册完成以后，到了用户管理界面，会有一个产品服务的选择购买，免费版3000次/天的足够非商城项目来使用了。购买完成后，如下展示的是可用的产品服务。

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fj9swdmym6j31kw0t9dtv.jpg)



### 物流跟踪

物流追踪API提供物流订单监控服务，用户将订单内容订阅到快递鸟后，快递鸟对订单进行实时监控，当物流轨迹有更新时，实时获取数据，对数据进行格式化，计算运单预计到达时间、全流程的物流状态、当前所在城市等数据后，推送给用户。监控直到订单签收后结束。

参照 [物流跟踪API文档](http://www.kdniao.com/api-follow)



### Yii2 - Code

因为用的是Yii2框架，根据官方的demo稍作修改，如下：



- 文件路径 `common/config/params-local.php`增加以下配置参数

  ```php
  // 快递鸟物流API接口参数
      'LOGISTICS' => [
          'EBusinessID' => '会员中心页 用户ID',
          'AppKey'      => '会员中心页 API key',
          'ReqURL'      => 'http://api.kdniao.cc/api/dist', // 快递鸟物流跟踪请求地址
          //'ReqURL'      => 'http://api.kdniao.cc/Ebusiness/EbusinessOrderHandle.aspx',  // 即时查询物流信息请求地址
      ],
  ```



- 后台填写好订单时，需要实现一个订阅快递鸟物流跟踪的方法，文件路径 `backend/controllers/KdniaoController.php`

```PHP
    public function actionSubscribe($requestData)
    {
    	// 调试用,调用此方法时需传递以下3个参数的值
        $requestData = [
            'OrderCode'    => '123123123123', // 商户订单号
            'ShipperCode'  => 'SF', // 物流渠道商名称
            'LogisticCode' => '123455672341', // 物流订单号
        ];

        $requestData = json_encode($requestData);

        $kdniao_params = Yii::$app->params[ 'LOGISTICS' ];
        ksort($kdniao_params, SORT_STRING);
        list($AppKey, $EBusinessID, $ReqURL) = array_values($kdniao_params);
        $kdniao = new KdniaoLogistic($AppKey, $EBusinessID, $ReqURL);

        return $kdniao->getOrderTracesByJson($requestData);
    }
```



- SDK文件路径 `common/sdk/KdniaoLogistic`

```php
<?php
namespace common\sdk;


/**
 * 快递鸟物流信息API
 *
 * Class KdniaoLogistic
 * @package common\sdk\
 */
class KdniaoLogistic
{
    protected $EBusinessID;
    protected $AppKey;
    protected $ReqURL;
    protected $RequestType;
    protected $DataType;


    /**
     * KdniaoLogistic constructor.
     * @param     $AppKey
     * @param     $EBusinessID
     * @param     $ReqURL
     * @param int $RequestType
     * @param int $DataType
     */
    public function __construct($AppKey, $EBusinessID, $ReqURL, $RequestType = 1008, $DataType = 2)
    {
        $this->EBusinessID = $EBusinessID; // 商户ID，请在我的服务页面查看。 // http://kdniao.com/UserCenter/Dev/Index.aspx
        $this->AppKey = $AppKey; // 商户KEY http://kdniao.com/UserCenter/Dev/Index.aspx
        $this->ReqURL = $ReqURL; // 接口地址
        $this->RequestType = $RequestType; // 请求指令类型
        $this->DataType = $DataType; // json数据格式
    }

    /**
     * Json方式 查询订单物流轨迹
     * @param $requestData
     * @return string
     */
    public function getOrderTracesByJson($requestData)
    {
        $datas = [
            'EBusinessID' => $this->EBusinessID,
            'RequestType' => $this->RequestType,
            'DataType'    => $this->DataType,
            'RequestData' => urlencode($requestData),
        ];
        $datas[ 'DataSign' ] = $this->encrypt($requestData, $this->AppKey);
        $result = $this->sendPost($this->ReqURL, $datas);

        return $result;
    }

    /**
     * @param $url '请求Url'
     * @param $datas '提交的数据'
     * @return string 'url响应返回的html'
     */
    protected function sendPost($url, $datas)
    {
        $temps = [];
        foreach ($datas as $key => $value) {
            $temps[] = sprintf('%s=%s', $key, $value);
        }
        $post_data = implode('&', $temps);
        $url_info = parse_url($url);
        if (empty($url_info[ 'port' ])) {
            $url_info[ 'port' ] = 80;
        }
        $httpheader = "POST " . $url_info[ 'path' ] . " HTTP/1.0\r\n";
        $httpheader .= "Host:" . $url_info[ 'host' ] . "\r\n";
        $httpheader .= "Content-Type:application/x-www-form-urlencoded\r\n";
        $httpheader .= "Content-Length:" . strlen($post_data) . "\r\n";
        $httpheader .= "Connection:close\r\n\r\n";
        $httpheader .= $post_data;
        $fd = fsockopen($url_info[ 'host' ], $url_info[ 'port' ]);
        fwrite($fd, $httpheader);
        $gets = "";
        $headerFlag = true;
        while (!feof($fd)) {
            if (($header = @fgets($fd)) && ($header == "\r\n" || $header == "\n")) {
                break;
            }
        }
        while (!feof($fd)) {
            $gets .= fread($fd, 128);
        }
        fclose($fd);

        return $gets;
    }

    /**
     * 电商Sign签名生成
     * @param data
     * @param appkey
     * @return string
     */
    protected function encrypt($data, $appkey)
    {
        /** DataSign */
        return urlencode(base64_encode(md5($data . $appkey)));
    }

}
```



调用订阅方法，成功后快递鸟接口返回

```json
{"EBusinessID":"1302872","UpdateTime":"2017-09-06 14:25:12","Success":true}
```



- 在 [官方订阅推送](http://kdniao.com/UserCenter/Dev/SubscribePush.aspx) 页面进行回调地址验证，当然回调地址记得设置为外网可访问URL地址

  [设置回调地址](http://kdniao.com/UserCenter/Dev/Index.aspx) 必须先在[推送](http://kdniao.com/UserCenter/Dev/SubscribePush.aspx)进行验证才可以正常配置。

![](https://ws4.sinaimg.cn/large/006tKfTcgy1fj9tw4btw2j31kw13ddv9.jpg)



- 在回调地址方法里设置入口并进行物流轨迹参数处理，文件路径 `callback/controllers/KdniaoController.php`

```PHP
public function actionIndex()
    {
        $push_params = Yii::$app->request->bodyParams;
	   /** 添加自己的业务逻辑代码 **/
        $data = $push_params[ 'Data' ];
        foreach ($data as $param) {
            if ($param[ 'Success' ] === true) {
                $logistic_code = array_get($param, 'LogisticCode');
                $state = array_get($param, 'State');
                $traces = array_get($param, 'Traces');
                $logistics_query = Logistics::findOne(['code' => $logistic_code]);
                if (!empty($logistics_query)) {
                    $logistics_query->state = $state;
                    $logistics_query->traces = $traces;
                    $logistics_query->save(false);
                }
            }
        }

  		// 必须返回的格式, 如不正确, 将不能进行回调配置
        $data = [
            "EBusinessID" => Yii::$app->params[ 'LOGISTICS' ][ 'EBusinessID' ],
            "UpdateTime"  => date('Y-m-d H:i:s'),
            "Success"     => true,
            "Reason"      => "",
        ];

        return json_encode($data);
    }

```

- 点击 生成请求报文 -> 调用

![](https://ws3.sinaimg.cn/large/006tKfTcgy1fj9vxp7sxuj31i418o155.jpg)



- 调用结果显示成功时，进行配置回调地址就OK了。

![](https://ws2.sinaimg.cn/large/006tKfTcgy1fj9u7slaxlj31gg0rwn02.jpg)