<div align="center">

## Database Access and Permissions


</div>

### Description

This lesson converts the application, applet, and servlet examples from Lesson 6 to write to and read from a database using JDBCTM. JDBC is the JavaTM database connectivity application programming interface (API) available in the Java® 2 Platform software.
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Monica Pawlan \(Copyright Sun Microsystems Inc\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/monica-pawlan-copyright-sun-microsystems-inc.md)
**Level**          |Advanced
**User Rating**    |4.5 (45 globes from 10 users)
**Compatibility**  |Java \(JDK 1\.1\)
**Category**       |[Databases/ JDBC](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/databases-jdbc__2-61.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/monica-pawlan-copyright-sun-microsystems-inc-database-access-and-permissions__2-1838/archive/master.zip)





### Source Code

<p><font face="verdana,arial">The code for this lesson is very similar to the
code you saw in Lesson 6, but additional steps (beyond converting the file
access code to database access code) include setting up the environment,
creating a database table, and connecting to the database. Creating a database
table is a database administration task that is not part of your program code.
However, establishing a database connection and the resulting database access
are.</font>
<p><font face="verdana,arial">As in Lesson 6, the applet needs appropriate
permissions to connect to the database. Which permissions it needs varies with
the type of driver used to make the database connection.</font>
<ul>
 <li><a href="#setup"><font face="verdana,arial">Database Setup</font></a>
 <li><a href="#db"><font face="verdana,arial">Create Database Table</font></a>
 <li><a href="#app"><font face="verdana,arial">Database Access by Applications</font></a>
  <ul>
   <li><a href="#estab"><font face="verdana,arial">Establishing a Connection</font></a>
   <li><a href="#final"><font face="verdana,arial">Final and Private
    Variables</font></a>
   <li><a href="#rw"><font face="verdana,arial">Writing and Reading Data</font></a></li>
  </ul>
 <li><a href="#applet"><font face="verdana,arial">Database Access by Applets</font></a>
  <ul>
   <li><a href="#jdbc"><font face="verdana,arial">JDBC Driver</font></a>
   <li><a href="#odbc"><font face="verdana,arial">JDBC-ODBC Bridge with ODBC
    Driver</font></a></li>
  </ul>
 <li><a href="#serv"><font face="verdana,arial">Database Access by Servlets</font></a>
 <li><a href="#more"><font face="verdana,arial">More Information</font></a></li>
</ul>
<font face="Verdana, Arial, Helvetica, sans-serif">
<hr>
<a name="setup"></a></font>
<h3><font face="verdana,arial">Database Setup</font></h3>
<font face="verdana,arial">You need access to a database if you want to run the
examples in this lesson. You can install a database on your machine or perhaps
you have access to a database at work. Either way, you need a database driver
and any relevant environment settings so your program can load the driver and
locate the database. The program will also need database login information in
the form of a user name and password.</font>
<p><font face="verdana,arial">A database driver is software that lets a program
establish a connection with a database. If you do not have the right driver for
the database to which you want to connect, your program will be unable to
establish the connection.</font>
<p><font face="verdana,arial">Drivers either come with the database or are
available from the Web. If you install your own database, consult the
documentation for the driver for information on installation and any other
environment settings you need for your platform. If you are using a database at
work, consult your database administrator for this information.</font>
<p><font face="verdana,arial">To show you two ways to do it, the application
example uses the <code>jdbc</code> driver, the applet examples use the <code>jdbc</code>
and <code>jdbc.odbc</code> drivers, and the servlet example uses the <code>jdbc.odbc</code>
driver. All examples connect to an <code>OracleOCI7.3.4</code> database.</font>
<p><font face="verdana,arial">Connections to other databases will involve
similar steps and code. Be sure to consult your documentation or system
administrator if you need help connecting to the database. <a name="db"></a></font>
<h3><font face="verdana,arial">Create Database Table</font></h3>
<font face="verdana,arial">Once you have access to a database, create a table in
it for the examples in this lesson. You need a table with one text field for
storing character data.</font>
<pre><font face="verdana,arial">TABLE DBA (
    TEXT      varchar2(100),
    primary key (TEXT)
)
</font></pre>
<font face="Verdana, Arial, Helvetica, sans-serif"><a name="app"></a></font>
<h3><font face="verdana,arial">Database Access by Applications</font></h3>
<font face="verdana,arial">This example converts the <a href="http://www.planet-source-code.com/vb/tutorial/java/samples/FileIO.java">FileIO</a>
program from Lesson 6 to write data to and read data from a database. The top
window below appears when you start the <a href="http://www.planet-source-code.com/vb/tutorial/java/samples/Dba.java">Dba</a>
application, and the window beneath it appears when you click the <code>Click Me</code>
button.</font>
<p><font face="verdana,arial">When you click the <code>Click Me</code> button,
whatever is entered into the text field is saved to the database. After that,
the data is retrieved from the database and displayed in the window shown on the
bottom. If you write data to the table more than once, everything written is
read and displayed in the window shown on the bottom, so you might have to
enlarge the window to see the entire list of table items.</font>
<p><center><font face="verdana,arial"><img src="http://www.planet-source-code.com/vb/tutorial/java/images/dba1.gif" width="252" height="129"></font>
<p><font face="verdana,arial">When Application Starts</font>
<p><font face="verdana,arial"><img src="http://www.planet-source-code.com/vb/tutorial/java/images/dba2.gif" width="247" height="157"></font>
<p><font face="verdana,arial">After Writing Orange and Apple to Database</font></center>
<p><font face="verdana,arial">The database access application needs code to
establish the database connection and do the database read and write operations.
<a name="estab"></a></font>
<h4><font face="verdana,arial">Establishing a Database Connection</font></h4>
<font face="verdana,arial">The JDBC <code>DriverManager</code> class can handle
multiple database drivers, and initiates all database communication. To load the
driver and connect to the database, the application needs a <code>Connection</code>
object and <code>Strings</code> that represent the <code>_driver</code> and <code>_url</code>.</font>
<p><font face="verdana,arial">The <code>_url</code> string is in the form of a
Uniform Resource Locator (URL). It consists of the URL, Oracle subprotcol, and
Oracle data source in the form <code>jdbc:oracle:thin</code>, the database login
<code>username</code>, the <code>password</code>, plus machine, port, and
protocol information.</font>
<pre><font face="verdana,arial">private Connection c;
final static private String _driver =
 &quot;oracle.jdbc.driver.OracleDriver&quot;;
final static private String _url =
 &quot;jdbc:oracle:thin:username/password@(description=(
 address_list=(address=(protocol=tcp)
 (host=developer)(port=1521)))
 (source_route=yes)(connect_data=(sid=jdcsid)))&quot;;
</font></pre>
<p><font face="verdana,arial">The <code>actionPerformed</code> method calls the <code>Class.forName(_driver)</code>
method to load the driver, and the <code>DriverManager.getConnection</code>
method to establish the connection. The <a href="http://exhiisprod01/vb/tutorial/java/default.asp?lngWId=2&amp;txtTutorialName=FileAccessAndPermissions.asp#excep">Exception
Handling</a> section in Lesson 6 describes <code>try</code> and <code>catch</code>
blocks. The only thing different here is that this block uses two <code>catch</code>
statements because two different errors are possible.</font>
<p><font face="verdana,arial">The call to <code>Class.forName(_driver);</code>
throws <code>java.lang.ClassNotFoundException</code>, and the call to <code>c =
DriverManager.getConnection(_url);</code> throws <code>java.sql.SQLException</code>.
In the case of either error, the application tells the user what is wrong and
exits because the program cannot operate in any meaningful way without a
database driver or connection.</font>
<pre><font face="verdana,arial">public void actionPerformed(ActionEvent event){
 try{
//Load the driver
   Class.forName(_driver);
//Establish database connection
   c = DriverManager.getConnection(_url);
  }catch (java.lang.ClassNotFoundException e){
   System.out.println(&quot;Cannot find driver class&quot;);
   System.exit(1);
  }catch (java.sql.SQLException e){
   System.out.println(&quot;Cannot get connection&quot;);
   System.exit(1);
  }
</font></pre>
<font face="Verdana, Arial, Helvetica, sans-serif"><a name="final"></a></font>
<h4><font face="verdana,arial">Final and Private Variables</font></h4>
<font face="verdana,arial">The member variables used to establish the database
connection above are declared <code>private</code>, and two of those variables
are also declared <code>final</code>.</font>
<p><font face="verdana,arial"><strong>final</strong>: A <code>final</code>
variable contains a constant value that can never change once it is initialized.
In the example, the user name, and password are <code>final</code> variables
because you would not want to allow an instance of this or any other class to
change this information.</font>
<p><font face="verdana,arial"><strong>private</strong>: A <code>private</code>
variable can only be used (accessed) by the class in which it is declared. No
other class can read or change <code>private</code> variables. In the example,
the database driver, user name, and password variables are <code>private</code>
to prevent an outside class from accessing them and jeopardizing the database
connection, or compromising the secret user name and password information. You
can find more information on this in the <a href="http://java.sun.com/docs/books/tutorial/java/javaOO/index.html" target="_blank">Objects
and Classs</a> lesson in <a href="http://java.sun.com/docs/books/tutorial" target="_blank">The
Java Tutorial</a> <a name="rw"></a></font>
<h4><font face="verdana,arial">Writing and Reading Data</font></h4>
<font face="verdana,arial">In the write operation, a <code>Statement</code>
object is created from the <code>Connection</code>. The <code>Statement</code>
object has methods for executing SQL queries and updates. Next, a <code>String</code>
object that contains the SQL update for the write operation is constructed and
passed to the <code>executeUpdate</code> method of the <code>Statement</code>
object.</font>
<pre><font face="verdana,arial">Object source = event.getSource();
if(source == button){
 JTextArea displayText = new JTextArea();
 try{
//Code to write to database
  String theText = textField.getText();
  Statement stmt = c.createStatement();
  String updateString = &quot;INSERT INTO dba VALUES
			  ('&quot; + theText + &quot;')&quot;;
  int count = stmt.executeUpdate(updateString);
</font></pre>
<font face="verdana,arial">SQL commands are <code>String</code> objects, and
therefore, follow the rules of <code>String</code> construction where the string
is enclosed in double quotes (&quot; &quot;) and variable data is appended with
a plus (+). The variable <code>theText</code> has single and double quotes to
tell the database the SQL string has variable rather than literal data.</font>
<p><font face="verdana,arial">In the read operation, a <code>ResultSet</code>
object is created from the <code>executeQuery</code> method of the <code>Statement</code>
object. The <code>ResultSet</code> contains the data returned by the query. To
retrieve the data returned, the code iterates through the <code>ResultSet</code>,
retrieves the data, and appends the data to the text area, <code>displayText</code>.</font>
<pre><font face="verdana,arial">//Code to read from database
  ResultSet results = stmt.executeQuery(
	&quot;SELECT TEXT FROM dba &quot;);
  while(results.next()){
   String s = results.getString(&quot;TEXT&quot;);
   displayText.append(s + &quot;\n&quot;);
  }
  stmt.close();
 } catch(java.sql.SQLException e){
  System.out.println(e.toString());
 }
//Display text read from database
 panel.removeAll();
 panel.add(&quot;North&quot;, clicked);
 panel.add(&quot;Center&quot;, displayText);
 panel.add(&quot;South&quot;, clickButton);
 panel.validate();
 panel.repaint();
}
</font></pre>
<font face="Verdana, Arial, Helvetica, sans-serif"><a name="applet"></a></font>
<h3><font face="verdana,arial">Database Access by Applets</font></h3>
<font face="verdana,arial">The applet version of the example is like the
application code described above except for the standard differences between
applications and applets described in the <a href="http://exhiisprod01/vb/tutorial/java/default.asp?lngWId=2&amp;txtTutorialName=BuildingApplets.asp#struct">Structure
and Elements</a> section of Lesson 3.</font>
<p><font face="verdana,arial">However, if you run the applet without a policy
file, you get a stack trace indicating permission errors. The <a href="http://exhiisprod01/vb/tutorial/java/default.asp?lngWId=2&amp;txtTutorialName=FileAccessAndPermissions.asp#sec">Granting
Applets Permission</a> section in Lesson 6 introduced you to policy files and
how to launch an applet with the permission it needs. The Lesson 6 applet
example provided the policy file and told you how to launch the applet with it.
This lesson shows you how to read the stack trace to determine the permissions
you need in a policy file.</font>
<p><font face="verdana,arial">To keep things interesting, this lesson has two
versions of the database access applet: one uses the JDBC driver, and the other
uses the the JDBC-ODBC bridge with an Open DataBase Connectivity (ODBC) driver.</font>
<p><font face="verdana,arial">Both applets do the same operations to the same
database table using different drivers. Each applet has its own policy file with
different permission lists and has different requirements for locating the
database driver <a name="jdbc"></a></font>
<h4><font face="verdana,arial">JDBC Driver</font></h4>
<font face="verdana,arial">The JDBC driver is used from a program written
exclusively in the Java language (Java program). It converts JDBC calls directly
into the protocol used by the DBMS. This type of driver is available from the
DBMS vendor and is usually packaged with the DBMS software.</font>
<p><font face="verdana,arial"><strong>Starting the Applet:</strong> To
successfully run, the <a href="http://www.planet-source-code.com/vb/tutorial/java/samples/DbaAppl.java">DbaAppl.java</a>
applet needs an available database driver and a policy file. This section walks
through the steps to get everything set up. Here is the <code>DbaAppl.html</code>
file for running the <code>DbaAppl</code> applet:</font>
<pre><font face="verdana,arial">&lt;HTML&gt;
&lt;BODY&gt;
&lt;APPLET CODE=DbaAppl.class
 WIDTH=200
 HEIGHT=100&gt;
&lt;/APPLET&gt;
&lt;/BODY&gt;
&lt;/HTML&gt;
</font></pre>
<p><font face="verdana,arial">And here is how to start the applet with
appletviewer:</font>
<pre><font face="verdana,arial"> appletviewer DbaAppl.html
</font></pre>
<p><font face="verdana,arial"><strong>Locating the Database Driver:</strong>
Assuming the driver is not available to the <code>DriverManager</code> for some
reason, the following error generates when you click the <code>Click Me</code>
button.</font>
<pre><font face="verdana,arial"> cannot find driver
</font></pre>
<font face="verdana,arial">This error means the DriverManager looked for the
JDBC driver in the directory where the applet HTML and class files are and could
not find it. To correct this error, copy the driver to the directory where the
applet files are, and if the driver is bundled in a zip file, unzip the zip file
so the applet can access the driver.</font>
<p><font face="verdana,arial">Once you have the driver in place, launch the
applet again.</font>
<pre><font face="verdana,arial"> appletviewer DbaAppl.html
</font></pre>
<font face="verdana,arial"><a name="perm"></a><strong>Reading a Stack Trace:</strong>
Assuming the driver is locally available to the applet, if the <a href="http://www.planet-source-code.com/vb/tutorial/java/samples/DbaAppl.java">DbaAppl.java</a>
applet is launched without a policy file, the following stack trace is generated
when the end user clicks the <code>Click Me</code> button.</font>
<pre><font face="verdana,arial">java.security.AccessControlException: access denied
(java.net.SocketPermission developer resolve)
</font></pre>
<font face="verdana,arial">The first line in the above stack trace tells you
access is denied. This means this stack trace was generated because the applet
tried to access a system resource without the proper permission. The second line
means to correct this condition you need a <code>SocketPermission</code> that
gives the applet access to the machine (<code>developer</code>) where the
database is located.</font>
<p><font face="verdana,arial">You can use Policy tool to create the policy file
you need, or you can create it with an ASCII editor. Here is the policy file
with the permission indicated by the stack trace:</font>
<pre><font face="verdana,arial">grant {
 permission java.net.SocketPermission &quot;developer&quot;,
  &quot;resolve&quot;;
 &quot;accessClassInPackage.sun.jdbc.odbc&quot;;
};
</font></pre>
<p><font face="verdana,arial">Run the applet again, this time with a policy file
named <code>DbaApplPol</code> that has the above permission in it:</font>
<pre><font face="verdana,arial">appletviewer -J-Djava.security.policy=DbaApplPol
                    DbaAppl.html
</font></pre>
<font face="verdana,arial">You get a stack trace again, but this time it is a
different error condition.</font>
<pre><font face="verdana,arial"> java.security.AccessControlException: access denied
 (java.net.SocketPermission
 129.144.176.176:1521 connect,resolve)
</font></pre>
<font face="verdana,arial">Now you need a <code>SocketPermission</code> that
allows access to the Internet Protocol (IP) address and port on the <code>developer</code>
machine where the database is located.</font>
<p><font face="verdana,arial">Here is the <code>DbaApplPol</code> policy file
with the permission indicated by the stack trace added to it:</font>
<pre><font face="verdana,arial">grant {
 permission java.net.SocketPermission &quot;developer&quot;,
            &quot;resolve&quot;;
 permission java.net.SocketPermission
 &quot;129.144.176.176:1521&quot;, &quot;connect,resolve&quot;;
};
</font></pre>
<font face="verdana,arial">Run the applet again. If you use the above policy
file with the Socket permissions indicated, it works just fine.</font>
<pre><font face="verdana,arial"> appletviewer -J-Djava.security.policy=DbaApplPol
                     DbaAppl.html
</font></pre>
<font face="Verdana, Arial, Helvetica, sans-serif"><a name="odbc"></a></font>
<h4><font face="verdana,arial">JDBC-ODBC Bridge with ODBC Driver</font></h4>
<font face="verdana,arial">Open DataBase Connectivity (ODBC) is Microsoft's
programming interface for accessing a large number of relational databases on
numerous platforms. The JDBC-ODBC bridge is built into the Solaris and Windows
versions of the Java platform so you can do two things:</font>
<ol>
 <li><font face="verdana,arial">Use ODBC from a Java program</font><font face="Verdana, Arial, Helvetica, sans-serif">
  <p>&nbsp;</p>
  </font>
 <li><font face="verdana,arial">Load ODBC drivers as JDBC drivers. This example
  uses the JDBC-ODBC bridge to load an ODBC driver to connect to the database.
  The applet has no ODBC code, however.</font></li>
</ol>
<p><font face="verdana,arial">The <code>DriverManager</code> uses environment
settings to locate and load the database driver. For this example, the driver
file does not need to be locally accessible.</font>
<p><font face="verdana,arial"><strong>Start the Applet:</strong> Here is the <code>DbaOdb.html</code>
file for running the <code>DbaOdbAppl</code> applet:</font>
<pre><font face="verdana,arial">&lt;HTML&gt;
&lt;BODY&gt;
&lt;APPLET CODE=DbaOdbAppl.class
 WIDTH=200
 HEIGHT=100&gt;
&lt;/APPLET&gt;
&lt;/BODY&gt;
&lt;/HTML&gt;
</font></pre>
<font face="verdana,arial">And here is how to start the applet:</font>
<pre><font face="verdana,arial"> appletviewer DbaOdb.html
</font></pre>
<p><font face="verdana,arial"><strong>Reading a Stack Trace:</strong> If the <a href="http://www.planet-source-code.com/vb/tutorial/java/samples/DbaOdbAppl.java">DbaOdbAppl.java</a>
applet is launched without a policy file, the following stack trace is generated
when the end user clicks the <code>Click Me</code> button.</font>
<pre><font face="verdana,arial"> java.security.AccessControlException: access denied
 (java.lang.RuntimePermission
 accessClassInPackage.sun.jdbc.odbc )
</font></pre>
<font face="verdana,arial">The first line in the above stack trace tells you
access is denied. This means this stack trace was generated because the applet
tried to access a system resource without the proper permission. The second line
means you need a <code>RuntimePermission</code> that gives the applet access to
the <code>sun.jdbc.odbc</code> package. This package provides the JDBC-ODBC
bridge functionality to the Java<a href="#TJVM"><sup>1</sup></a> virtual machine
(VM).</font>
<p><font face="verdana,arial">You can use Policy tool to create the policy file
you need, or you can create it with an ASCII editor. Here is the policy file
with the permission indicated by the stack trace:</font>
<pre><font face="verdana,arial">grant {
 permission java.lang.RuntimePermission
  &quot;accessClassInPackage.sun.jdbc.odbc&quot;;
};
</font></pre>
<p><font face="verdana,arial">Run the applet again, this time with a policy file
named <code>DbaOdbPol</code> that has the above permission in it:</font>
<pre><font face="verdana,arial"> appletviewer -J-Djava.security.policy=DbaOdbPol
                     DbaOdb.html
</font></pre>
<font face="verdana,arial">You get a stack trace again, but this time it is a
different error condition.</font>
<pre><font face="verdana,arial"> java.security.AccessControlException:
 access denied (java.lang.RuntimePermission
 file.encoding read)
</font></pre>
<font face="verdana,arial">The stack trace means the applet needs read
permission to the encoded (binary) file. Here is the <code>DbaOdbPol</code>
policy file with the permission indicated by the stack trace added to it:</font>
<pre><font face="verdana,arial"> grant {
  permission java.lang.RuntimePermission
    &quot;accessClassInPackage.sun.jdbc.odbc&quot;;
  permission java.util.PropertyPermission
    &quot;file.encoding&quot;, &quot;read&quot;;
 };
</font></pre>
<font face="verdana,arial">Run the applet again. If you use the above policy
file with the Runtime and Property permissions indicated, it works just fine.</font>
<pre><font face="verdana,arial"> appletviewer -J-Djava.security.policy=DbaOdbPol
                     DbaOdb.html
</font></pre>
<font face="Verdana, Arial, Helvetica, sans-serif"><a name="serv"></a></font>
<h3><font face="verdana,arial">Database Access by Servlets</font></h3>
<font face="verdana,arial">As you learned in Lesson 6, servlets are under the
security policy in force for the web server under which they run. When the
database read and write code is added to the <code>FileIOServlet</code> from
Lesson 6, the <a href="http://www.planet-source-code.com/vb/tutorial/java/samples/DbaServlet.java">DbaServlet.java</a>
servlet for this lesson executes without restriction under Java WebServer<font size="-2"><sup>TM</sup></font>
1.1.1.</font>
<p><font face="verdana,arial">The web server has to be configured to locate the
database. Consult your web server documentation or database administrator for
help. With Java WebServer 1.1.1, the configuration setup involves editing the
startup scripts with such things as environment settings for loading the ODBC
driver, and locating and connecting to the database.</font>
<p><font face="verdana,arial"><img src="http://www.planet-source-code.com/vb/tutorial/java/images/dba1.gif" width="252" height="129"></font>
<table border="0" cellPadding="2" cellSpacing="2">
 <tbody>
  <tr>
   <td><font face="verdana,arial"><img src="http://www.planet-source-code.com/vb/tutorial/java/images/dba2.gif" width="247" height="157"></font></td>
  </tr>
 </tbody>
</table>
<pre><font face="verdana,arial">import java.io.*;
import javax.servlet.*;
import javax.servlet.http.*;
import java.sql.*;
import java.net.*;
import java.io.*;
public class DbaServlet extends HttpServlet {
 private Connection c;
 final static private String _driver =
	&quot;sun.jdbc.odbc.JdbcOdbcDriver&quot;;
 final static private String _user = &quot;username&quot;;
 final static private String _pass = &quot;password&quot;;
 final static private String
			_url = &quot;jdbc:odbc:jdc&quot;;
public void doPost(HttpServletRequest request,
	HttpServletResponse response)
	throws ServletException, IOException{
 response.setContentType(&quot;text/html&quot;);
 PrintWriter out = response.getWriter();
 out.println(&quot;&lt;title&gt;Example&lt;title&gt;&quot; +
   &quot;&lt;body bgcolor=FFFFFF&gt;&quot;);
 out.println(&quot;&lt;h2&gt;Button Clicked&lt;/h2&gt;&quot;);
 String DATA = request.getParameter(&quot;DATA&quot;);
 if(DATA != null){
   out.println(&quot;&lt;STRONG&gt;Text from
			form:&lt;/STRONG&gt;&quot;);
   out.println(DATA);
 } else {
  out.println(&quot;No text entered.&quot;);
 }
//Establish database connection
 try{
  Class.forName (_driver);
  c = DriverManager.getConnection(_url,
				  _user,
				  _pass);
  } catch (Exception e) {
   e.printStackTrace();
   System.exit(1);
  }
  try{
//Code to write to database
   Statement stmt = c.createStatement();
   String updateString = &quot;INSERT INTO dba &quot; +
		&quot;VALUES ('&quot; + DATA + &quot;')&quot;;
   int count = stmt.executeUpdate(updateString);
//Code to read from database
   ResultSet results = stmt.executeQuery(
		&quot;SELECT TEXT FROM dba &quot;);
   while(results.next()){
    String s = results.getString(&quot;TEXT&quot;);
    out.println(&quot;&lt;BR&gt;
     &lt;STRONG&gt;Text from database:&lt;/STRONG&gt;&quot;);
    out.println(s);
   }
   stmt.close();
   }catch(java.sql.SQLException e){
   System.out.println(e.toString());
   }
   out.println(&quot;&lt;P&gt;Return to
    &lt;A HREF=&quot;../dbaHTML.html&quot;&gt;Form&lt;/A&gt;&quot;);
   out.close();
 }
}
</font></pre>
<font face="Verdana, Arial, Helvetica, sans-serif"><a name="more"></a></font>
<h3><font face="verdana,arial">More Information</font></h3>
<font face="verdana,arial">You can find more information on variable access
settings in the <a href="http://java.sun.com/docs/books/tutorial/java/javaOO/index.html" target="_blank">Objects
and Classes</a> trail in <a href="http://java.sun.com/docs/books/tutorial" target="_blank">The
Java Tutorial</a></font>
<p><font face="verdana,arial">_______<br>
<a name="TJVM"><sup>1</sup></a> As used on this web site, the terms &quot;Java
virtual machine&quot; or &quot;JVM&quot; mean a virtual machine for the Java
platform.<br>
</font>
<hr>
<!--BEGIN READER SURVEY-->
<font size="2">
<p><font face="verdana,arial"><b>Reprinted with permission from the <a href="http://developer.java.sun.com/developer/" target="_blank">Java
Developer Connection(SM)</a><br>
Copyright <a href="http://www.sun.com" target="_blank">Sun Microsystems Inc</a>.</b></font></p>
</font>

