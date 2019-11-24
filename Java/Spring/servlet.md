# servlet
## 定义
Servlet是运行在服务器端的帮助处理网络请求的一段代码，Tomcat是Servlet容器，像永动机一样时刻监听来自网络的请求，`Tomcat = Web服务器 + Servlet容器`。如果想实现一个能在Servlet容器中运行的服务端程序，也就是想让自己的业务代码在Servlet容器中运行，需要Tomcat去识别并调用，遵循Servlet规范。Servlet的核心是一套规范，大家都按照一个确定的规则行事。

下面的内容关系如下：
1. JDK提供了Servlet接口。
2. GenericServlet实现了Servlet接口，abstract。
3. HttpServlet继承了GenericServlet接口，依然是abstract，开发者只需要继承HttpServlet抽象类，重写相关方法，就可以写自己的业务逻辑了。
4. LocalServlet相当于一个继承HttpServlet类实现业务逻辑的案例。
## Servlet规范与Servlet接口
Servlet接口定义如下：
```java
package javax.servlet;

import java.io.IOException;

public interface Servlet {
    void init(ServletConfig var1) throws ServletException;

    ServletConfig getServletConfig();

    void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    String getServletInfo();

    void destroy();
}
```
只要实现这个接口，就代表遵循了Servlet规范。
## GenericServlet
jdk中提供了GenericServlet，不过可以看出，GenericServlet仅仅是一个抽象方法，做的工作也仅仅是将servletConfig从局部变量提升到全局变量，这样做的目的是方便在其他方法中读取到Servlet配置信息中的值，但是没有实现最核心的service方法。（注：原来的方法中仅仅只可以通过getServletConfig()方法获得，声明为全局变量后就可以直接使用servletConfig了。）
```java
package javax.servlet;

public abstract class GenericServlet implements Servlet, ServletConfig, Serializable {
    private transient ServletConfig config;

    public void init(ServletConfig config) throws ServletException {
        this.config = config;
        this.init();
    }

    public void init() throws ServletException {
    }

    public ServletConfig getServletConfig() {
        return this.config;
    }

    public abstract void service(ServletRequest var1, ServletResponse var2) throws ServletException, IOException;

    public String getServletInfo() {
        return "";
    }
}
```
## HttpServlet
继承自GenericServlet类，继承了init()和getServletConfig()方法。Service方法中，根据请求信息，拿到请求方式，根据请求方式封装了doGet，doPost，doPut等方法。作用是分析了根据请求是什么，去调用不同的方法。
```java
package javax.servlet.http;
public abstract class HttpServlet extends GenericServlet {
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        HttpServletRequest request;
        HttpServletResponse response;
        try {
            request = (HttpServletRequest)req;
            response = (HttpServletResponse)res;
        } catch (ClassCastException var6) {
            throw new ServletException(lStrings.getString("http.non_http"));
        }

        this.service(request, response);
    }
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String method = req.getMethod();
        long lastModified;
        if (method.equals("GET")) {
            lastModified = this.getLastModified(req);
            if (lastModified == -1L) {
                this.doGet(req, resp);
            } else {
                long ifModifiedSince;
                try {
                    ifModifiedSince = req.getDateHeader("If-Modified-Since");
                } catch (IllegalArgumentException var9) {
                    ifModifiedSince = -1L;
                }

                if (ifModifiedSince < lastModified / 1000L * 1000L) {
                    this.maybeSetLastModified(resp, lastModified);
                    this.doGet(req, resp);
                } else {
                    resp.setStatus(304);
                }
            }
        } else if (method.equals("HEAD")) {
            lastModified = this.getLastModified(req);
            this.maybeSetLastModified(resp, lastModified);
            this.doHead(req, resp);
        } else if (method.equals("POST")) {
            this.doPost(req, resp);
        } else if (method.equals("PUT")) {
            this.doPut(req, resp);
        } else if (method.equals("DELETE")) {
            this.doDelete(req, resp);
        } else if (method.equals("OPTIONS")) {
            this.doOptions(req, resp);
        } else if (method.equals("TRACE")) {
            this.doTrace(req, resp);
        } else {
            String errMsg = lStrings.getString("http.method_not_implemented");
            Object[] errArgs = new Object[]{method};
            errMsg = MessageFormat.format(errMsg, errArgs);
            resp.sendError(501, errMsg);
        }

    }
}
```
下面的方法中直接返回了错误码，如果继承了HttpServlet，但是什么也不写，GET请求会得到一个405，需要根据具体的业务需求重写doGet()和doPost等方法。
```java
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String protocol = req.getProtocol();
    String msg = lStrings.getString("http.method_get_not_supported");
    if (protocol.endsWith("1.1")) {
        resp.sendError(405, msg);
    } else {
        resp.sendError(400, msg);
    }

}
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    String protocol = req.getProtocol();
    String msg = lStrings.getString("http.method_post_not_supported");
    if (protocol.endsWith("1.1")) {
        resp.sendError(405, msg);
    } else {
        resp.sendError(400, msg);
    }
}
```
想要实现Servlet规范，并不需要从头开始每个方法都去实现一遍，如果需要的是处理Http请求，我们只需要继承HttpServlet，重写doGet等方法，去实现不同的请求逻辑即可。
## DefaultServlet
一个真正的Servlet，顾名思义，缺省servlet，是专门处理静态文件的一个Servlet，是默认在tomcat的wex.xml中配置的，当项目出现静态资源加载不出来的时候就要考虑是不是项目的web.xml配置的servlet拦截了这个DefaultServlet的工作。
```java
package org.apache.catalina.servlets;

public class DefaultServlet extends HttpServlet {

public void init() throws ServletException {
    if (this.getServletConfig().getInitParameter("debug") != null) {
        this.debug = Integer.parseInt(this.getServletConfig().getInitParameter("debug"));
    }

    if (this.getServletConfig().getInitParameter("input") != null) {
        this.input = Integer.parseInt(this.getServletConfig().getInitParameter("input"));
    }

    if (this.getServletConfig().getInitParameter("output") != null) {
        this.output = Integer.parseInt(this.getServletConfig().getInitParameter("output"));
    }

    this.listings = Boolean.parseBoolean(this.getServletConfig().getInitParameter("listings"));
    if (this.getServletConfig().getInitParameter("readonly") != null) {
        this.readOnly = Boolean.parseBoolean(this.getServletConfig().getInitParameter("readonly"));
    }

    this.compressionFormats = this.parseCompressionFormats(this.getServletConfig().getInitParameter("precompressed"), this.getServletConfig().getInitParameter("gzip"));
    if (this.getServletConfig().getInitParameter("sendfileSize") != null) {
        this.sendfileSize = Integer.parseInt(this.getServletConfig().getInitParameter("sendfileSize")) * 1024;
    }

    this.fileEncoding = this.getServletConfig().getInitParameter("fileEncoding");
    if (this.fileEncoding == null) {
        this.fileEncodingCharset = Charset.defaultCharset();
        this.fileEncoding = this.fileEncodingCharset.name();
    } else {
        try {
            this.fileEncodingCharset = B2CConverter.getCharset(this.fileEncoding);
        } catch (UnsupportedEncodingException var2) {
            throw new ServletException(var2);
        }
    }

    if (this.getServletConfig().getInitParameter("useBomIfPresent") != null) {
        this.useBomIfPresent = Boolean.parseBoolean(this.getServletConfig().getInitParameter("useBomIfPresent"));
    }

    this.globalXsltFile = this.getServletConfig().getInitParameter("globalXsltFile");
    this.contextXsltFile = this.getServletConfig().getInitParameter("contextXsltFile");
    this.localXsltFile = this.getServletConfig().getInitParameter("localXsltFile");
    this.readmeFile = this.getServletConfig().getInitParameter("readmeFile");
    if (this.getServletConfig().getInitParameter("useAcceptRanges") != null) {
        this.useAcceptRanges = Boolean.parseBoolean(this.getServletConfig().getInitParameter("useAcceptRanges"));
    }

    if (this.input < 256) {
        this.input = 256;
    }

    if (this.output < 256) {
        this.output = 256;
    }

    if (this.debug > 0) {
        this.log("DefaultServlet.init:  input buffer size=" + this.input + ", output buffer size=" + this.output);
    }

    this.resources = (WebResourceRoot)this.getServletContext().getAttribute("org.apache.catalina.resources");
    if (this.resources == null) {
        throw new UnavailableException(sm.getString("defaultServlet.noResources"));
    } else {
        if (this.getServletConfig().getInitParameter("showServerInfo") != null) {
            this.showServerInfo = Boolean.parseBoolean(this.getServletConfig().getInitParameter("showServerInfo"));
        }

        if (this.getServletConfig().getInitParameter("sortListings") != null) {
            this.sortListings = Boolean.parseBoolean(this.getServletConfig().getInitParameter("sortListings"));
            if (this.sortListings) {
                boolean sortDirectoriesFirst;
                if (this.getServletConfig().getInitParameter("sortDirectoriesFirst") != null) {
                    sortDirectoriesFirst = Boolean.parseBoolean(this.getServletConfig().getInitParameter("sortDirectoriesFirst"));
                } else {
                    sortDirectoriesFirst = false;
                }

                this.sortManager = new DefaultServlet.SortManager(sortDirectoriesFirst);
            }
        }

        if (this.getServletConfig().getInitParameter("allowPartialPut") != null) {
            this.allowPartialPut = Boolean.parseBoolean(this.getServletConfig().getInitParameter("allowPartialPut"));
        }

    }
}
```
上面的init()方法主要初始化一些内容，首先通过getServletConfig来获得我们的全局servletConfig，这里面定义了很多内容，值得注意的有fileEncoding，定义了字符编码，input,output定义了输入和输出的缓冲区大小。下面具体看一下service，doGet，doPost。
```java
protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    if (req.getDispatcherType() == DispatcherType.ERROR) {
        this.doGet(req, resp);
    } else {
        super.service(req, resp);
    }

}
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    this.serveResource(request, response, true, this.fileEncoding);
}
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException, ServletException {
    this.doGet(request, response);
}
```
调用了ServResource方法，根据请求信息中的资源路径，去项目下具体的位置获取静态资源。doPost还是调用的doGet方法

总结：
1. 要想实现一个能在servlet容器中运行的服务端程序，需要Tomcat去识别并调用，所以应该遵循servlet规范
2. 实现servlet规范只需要实现jdk提供的servlet接口即可
3. servlet接口中方法很多，我们如果直接实现的话比较繁琐
4. jdk提供了GenericServlet，实现了servlet接口，把servletConfig从局部变量提升为全局变量，方便在其他方法中读取到Servlet配置信息中的值，但是没有实现最核心的service方法
5. Tomact提供的httpServlet，继承自GenericServlet，对service方法进行了处理，根据请求方式封装了doGet,doPost,doPut等方法
6. 实现servlet规范就只需要继承自HttpServlet，重写doGet等方法即可
7. 以tomcat默认的静态资源处理DefaultServlet为例看一下servlet实现的具体步骤
## 参考
[你真的了解Servlet吗？](https://zhuanlan.zhihu.com/p/81726738)

[初学Java Web(4)——Servlet学习总结](https://www.cnblogs.com/wmyskxz/p/8804447.html)

[Servlet第一篇【介绍Servlet、HTTP协议、WEB目录结构、编写入门Servlet程序、Servlet生命周期】](https://mp.weixin.qq.com/s?__biz=MzI4Njg5MDA5NA==&mid=2247483680&idx=3&sn=d5380ff58c5077271ac9c43d2d96f6c1&chksm=ebd74021dca0c93733255324df8c1e522dbe36ccaf8c2c4bcca4765113a120eb9851ca0e2442#rd)