使用方法

1、 请先填写相关配置：在Config.properties里 client_ID ：appkey 创建应用获取到的appkey client_SERCRET ：app_secret 创建应用获取到的appsecret redirect_URI : 回调地址 OAuth2的回调地址

2、 然后调用example里：OAuth4Code.java

public class OAuth4Code {

	public static void main(String [] args) throws WeiboException, IOException{
		Oauth oauth = new Oauth();
		BareBonesBrowserLaunch.openURL(oauth.authorize("code"));
		System.out.print("Hit enter when it's done.[Enter]:");
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

		String code = br.readLine();
		Log.logInfo("code: " + code);
		try{
			System.out.println(oauth.getAccessTokenByCode(code));
		} catch (WeiboException e) {
			if(401 == e.getStatusCode()){
				Log.logInfo("Unable to get the access token.");
			}else{
				e.printStackTrace();
			}
		}
	}
}

3、 运行后会弹出浏览器地址跳转到授权认证页面，然后输入你的微博帐号和密码，会调转到你的回调地址页面，url后面会传递code参数

4、 然后在console输入code就能获取到oauth2的accesstoken

5、 接下来即可调用example，在此以user／show接口为例：

public class ShowUser {

	public static void main(String[] args) {
		String access_token = WeiboConfig.getValue("access_token");
		String uid = args[0];
		Users um = new Users(access_token);
		try {
			User user = um.showUserById(uid);
			Log.logInfo(user.toString());
		} catch (WeiboException e) {
			e.printStackTrace();
		}
	}
}

access_token为auth4code获取到的oauth2的accesstoken。
由于目前只开放支持code的oauth认证方式，所以sdk暂时只支持code获取token方式。


