# OwinSelfHostedWebApiWithMongoDB

This project can be exported and saved as a template in Visual Studio 2013. The template can be used to create a Console app, that will self-host Web API using OWIN self-hosting. It also uses MongoDB.AspNet.Identity to enable ASP.NET identity provider that uses MongoDB for storage.

## How to install the template
The template is already exported in the TemplateFile folder. You can copy paste the zip file into "Visual Studio 2013\My Exported Templates" folder" (by default it's in your Documents folder)
You can also open this project in Visual Studio, and export it yourself by going to "File->Export Templates".

## How to use the template project
1. File->New->Project
2. Select OwinSelfHostedWebApiWithMongoDB from the list of templates. Give the project a name and hit OK.
3. You can run the project and as a test go to your browser and enter http://localhost:9000/api/values in the address bar. You should get the message "Authorization has been denied for this request"

#### Register a new user
You can register a new user by posting to http://localhost:9000/api/account/register. The body of the message could be a JSON message that looks like the following:
```
{
  "Email": "test@test.com",
  "Password": "sample string 2",
  "ConfirmPassword": "sample string 2"
}
```

#### How to login
1- To authorize, first we need to obtain a token by posting to http://localhost:9000/token with a body like the following:
```
Password=sample string 2&UserName=test@test.com&grant_type=password
```
2- Now for every call that requires authorization, use the access_token and add the following header to the request:
```
Authorization: Bearer <access_token>
```

#### CORS
If you're planning to access the service from a different domain you need to enable CORS.

1- Install Microsoft.AspNet.WebApi.Cors:
```
Install-Package Microsoft.AspNet.WebApi.Cors
```
2- To enable CORS across all the methods in the service, add the following two lines to WebApiStartup.cs:
```
var cors = new EnableCorsAttribute("*", "*", "*");
config.EnableCors(cors);
```
You can optionally enable cors using attributes for different controllers.

3- Add the following line as the first line of GranResourceOwnerCredentials method in the ApplicationOAuthProvider.cs. This is separate from WebAPI and is necessary for OAuth:
```
context.OwinContext.Response.Headers.Add("Access-Control-Allow-Origin", new[] { "*" });
```
4- (optional) If you are using AngularJS, as the client, you may need to configure $httpProvider. You can follow the instructions here to do so: http://www.codeproject.com/Articles/742532/Using-Web-API-Individual-User-Account-plus-CORS-En

#### References
- This is an excellent video which briefly explains authorization in Web API: http://pluralsight.com/training/Player?author=scott-allen&name=aspdotnet-mvc5-fundamentals-m5-webapi2&mode=live&clip=0&course=aspdotnet-mvc5-fundamentals
- This project uses MongoDB.AspNet.Identity which is available here: https://github.com/InspectorIT/MongoDB.AspNet.Identity
- An excellent article on how to setup Web API 2 with AngularJS: http://www.codeproject.com/Articles/742532/Using-Web-API-Individual-User-Account-plus-CORS-En
