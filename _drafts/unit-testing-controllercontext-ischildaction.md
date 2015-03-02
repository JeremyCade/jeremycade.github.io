---
layout: post
title:  "Unit Testing ControllerContext.IsChildAction = true"
comments: true
categories: asp.net mvc unit testing 
---

In Asp.net MVC I often create Action Methods that return both a PartialViewResult and a ViewResult depending on how the method was called. 

## The Test Logic
- If requested via ajax, return a PartialViewResult.
- If requested as a "Child Action" return a PartialViewResult. 
- If requested normally e.g. via Http Get return a ViewResult.

I'm using xUnit as the testing framework, combined with Moq.
I am also making use of  

## The Tests
	[Fact]
	public void Task_IndexShouldReturnPartialViewResutForAjaxCalls() 
	{
	    // Arrange
	    var user = Mock.Get(this.controller.HttpContext.User);
	    user.Setup(x => x.Identity.Name).Returns("jeremy@aussieweb.com.au");
	    user.Setup(x => x.Identity.IsAuthenticated).Returns(true);

	    var userProfiles = Mock.Get(this.membership.UserProfiles);
	    userProfiles.Returns(MoqData.UserProfile);

	    // Add Ajax Request Headers
	    NameValueCollection headers = new NameValueCollection();
	    headers.Add("X-Requested-With", "XMLHttpRequest");
	    var request = Mock.Get(this.controller.Request);
	    request.Setup(x => x.Headers).Returns(headers);            

	    // Act
	    var result = this.controller.Index();

	    // Assert
	    Assert.IsType<PartialViewResult>(result);
	}

	[Fact]
	public void Task_IndexShouldReturnPartialViewResutForChildAction()
	{
	    // Arrange
	    var user = Mock.Get(this.controller.HttpContext.User);
	    user.Setup(x => x.Identity.Name).Returns("user@email.com");
	    user.Setup(x => x.Identity.IsAuthenticated).Returns(true);

	    var userProfiles = Mock.Get(this.membership.UserProfiles);
	    userProfiles.Returns(MoqData.UserProfile);

	    // Setup IsChildAction
	    RouteData routeData = new RouteData();
	    routeData.DataTokens["ParentActionViewContext"] = new ViewContext();
	    this.controller.ControllerContext.RouteData = routeData;

	    // Act
	    var result = this.controller.Index();

	    // Assert
	    Assert.IsType<PartialViewResult>(result);
	}

	[Fact]
	public void Task_IndexReturnsViewResult()
	{
	    // Arrange
	    var user = Mock.Get(this.controller.HttpContext.User);
	    user.Setup(x => x.Identity.Name).Returns("jeremy@aussieweb.com.au");
	    user.Setup(x => x.Identity.IsAuthenticated).Returns(true);

	    var userProfiles = Mock.Get(this.membership.UserProfiles);
	    userProfiles.Returns(MoqData.UserProfile);

	    // Act
	    var result = this.controller.Index();

	    // Assert
	    Assert.IsType<ViewResult>(result);
	}

## Test Helpers
	public static HttpContextBase FakeHttpContext()
	{            
	    var context = new Mock<HttpContextBase>();            
	    var request = new Mock<HttpRequestBase>();            
	    var response = new Mock<HttpResponseBase>();
	    var session = new Mock<HttpSessionStateBase>();
	    var server = new Mock<HttpServerUtilityBase>();            
	    var user = new Mock<IPrincipal>(); 
	    var identity = new Mock<IIdentity>();

            
	    context.Setup(ctx => ctx.Request).Returns(request.Object);
	    context.Setup(ctx => ctx.Response).Returns(response.Object);
	    context.Setup(ctx => ctx.Session).Returns(session.Object);
	    context.Setup(ctx => ctx.Server).Returns(server.Object);
	    context.Setup(ctx => ctx.User).Returns(user.Object);
	    context.Setup(ctx => ctx.User.Identity).Returns(identity.Object);

	    return context.Object;
	}

	public static HttpContextBase FakeHttpContext(String url)
	{
	    HttpContextBase context = FakeHttpContext();
	    context.Request.SetupRequestUrl(url);
	    return context;
	}

	public static void SetFakeControllerContext(this Controller controller)
	{
	    var httpContext = FakeHttpContext();                        
	    ControllerContext context = new ControllerContext(new RequestContext(httpContext, new RouteData()), controller);                      

	    controller.ControllerContext = context;
            
	}

## The Action Method
public ActionResult Index()
{
	var tasks = this.GetTasks();

	// Return partial view if called via Ajax or Html.Action()
	// from a view.
	if (this.Request.IsAjaxRequest() || this.ControllerContext.IsChildAction)
	{
	    return this.PartialView("_list", tasks);
	}

	TasksModel model = new TasksModel();
	model.Filter = string.Empty;
	model.Tasks = tasks;
	return this.View(model);
}