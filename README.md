# 学习通签到Api分析  
### 登录接口  
#### 账号密码登录1
    https://passport2-api.chaoxing.com/v11/loginregister?code=${pwd}&cx_xxt_passport=json&uname=${uname}&loginType=1&roleSelect=true
> 接口返回`cookie`和`uid` 
#### 账号密码登录2
    https://passport2.chaoxing.com/fanyalogin
>Post提交`fid=-1&uname=${uname}&password=${encryptPwd}&refer=https%253A%252F%252Fwww.baidu.com%252Flink%253Furl%253D7F6K1ISfp_Qh_YMOftV_1CdfwkA8zQnhOR6jlqtCVZxdMssUZVIX2uVSC1NXiSebyQ8Ur8YILmFm0Vo7naeSl_%2526wd%253D%2526eqid%253Df3f8f74c0023361200000003634e4a8b&t=true&forbidotherlogin=0&validate=&doubleFactorLogin=0&independentId=0`
> > `encryptPwd`是DES加密后的密码 mode:ECB   padding:Pkcs7   iv:u2oh6Vu^HWe40fj
> > > 接口返回`cookie`和`uid` 
#### 二维码扫码登录  
    http://passport2.chaoxing.com/login?fid=&newversion=true&refer=http://i.chaoxing.com
> 页面返回`uuid`和`enc`  
 
    http://passport2.chaoxing.com/createqr?uuid=${uuid}&fid=-1
> 页面返回`二维码图片`

    http://passport2.chaoxing.com/getauthstatus
> Post提交`enc=${enc}&uuid=${uuid}`
> > 接口返回扫码状态，登陆成功协议头返回`cookie`和`uid`  

### 获取课程接口 
    http://mooc1-api.chaoxing.com/mycourse/backclazzdata?view=json&rss=1
> 接口返回`courseId`和`classId`    

### 查询活动接口 
#### 查询活动接口1
    https://mobilelearn.chaoxing.com/v2/apis/active/student/activelist?fid=0&courseId=${courseId}&classId=${classId}&showNotStartedActive=0&_=1663752482576
> 接口返回`activeId`
#### 查询活动接口2
    https://mobilelearn.chaoxing.com/ppt/activeAPI/taskactivelist?courseId=${courseId}&classId=${classId}
> 须在请求头加入学习通原生UA`User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_0_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148 (schild:eaf4fb193ec970c0a9775e2a27b0232b) (device:iPhone11,2) Language/zh-Hans com.ssreader.ChaoXingStudy/ChaoXingStudy_3_6.0.2_ios_phone_202209281930_99 (@Kalimdor)_1665876591620212942`
> > 接口返回`activeId`
### 签到接口
#### 预签到（所有签到接口访问前都必须进行一次预签到）
    https://mobilelearn.chaoxing.com/newsign/preSign?courseId=${courseId}&classId=${classId}&activePrimaryId=${activeId}&general=1&sys=1&ls=1&appType=15&tid=&uid=${uid}&ut=s
#### 通用签到接口(普通签到,手势签到，签到码签到，不提交图片的拍照签到，不提交位置的位置签到)
    https://mobilelearn.chaoxing.com/pptSign/stuSignajax?activeId=${activeId}
##### 手势签到，签到码签到的签到码获取
    https://mobilelearn.chaoxing.com/v2/apis/active/getPPTActiveInfo?activeId=${activeId}
> 接口返回的`signcode`项是签到码
#### 提交图片的拍照签到
    https://mobilelearn.chaoxing.com/pptSign/stuSignajax?activeId=${activeId}&objectId=${objectId}
##### 获取objectId
###### 登录超星网盘
    https://pan-yz.chaoxing.com/api/token/uservalid   
> 接口返回网盘`token` 
###### 上传图片
    https://pan-yz.chaoxing.com/upload  
>  post提交`{'puid': uid, '_token': token}`以及需要上传的`图片`    
>> 接口返回`objectId`
>>> 你可以在`https://p.ananas.chaoxing.com/star3/270_160c/${objectId}.png`预览你的图片
#### 自定义位置的位置签到
    https://mobilelearn.chaoxing.com/pptSign/stuSignajax?address=${locationText}&activeId=${activeId}&latitude=${locationLatitude}&longitude=${locationLongitude}&fid=0&appType=15&ifTiJiao=1
> `locationText` 是签到位置 `locationLatitude`是签到位置的纬度  `locationLongitude`是签到位置的经度
##### 获取教师指定位置签到的所需信息
    https://mobilelearn.chaoxing.com/v2/apis/active/getPPTActiveInfo?activeId=${activeId}
>接口返回所需要的 `locationText` , `locationLatitude`,  `locationLongitude`
#### 二维码签到
    https://mobilelearn.chaoxing.com/pptSign/stuSignajax?activeId=${activeId}&enc=${enc}&fid=0
>`enc`参数为签到二维码解码所得
   
