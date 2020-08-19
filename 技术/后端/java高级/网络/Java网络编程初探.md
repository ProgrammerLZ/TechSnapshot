

#Java网络编程

# 初探



## Networking Concepts
Java Networking is a concept of connecting two or more computing devices together so that we can share resources.  
Java **socket programming** provides facility to share data between different computing devices.  

### Advantage of Java Networking
* sharing resources
* centralize software management

### Java Networking Terminology
* IP Address
* Protocol
* Port Number
* MAC Address
* Connection-oriented and connection-less protocol
* Socket

####1.IP Adress
**IP address** is a unique number assigned to a node of a network e.g. 192.168.0.1 . It is composed of octets(八重奏) that range from 0 to 255.  
It is a logical address that can be changed.

####2.Protocol
A **protocol** is a set of rules basically that is followed for communication. For example:  

* TCP
* FTP
* Telnet
* SMTP
* POP etc.

####3.Port Number
The **port number** is used to uniquely identify different applications. It acts as a communication endpoint between applications.  
The port number is associated with the IP address for communication between two applications.

####4.MAC Adress
MAC (Media Access Control) Address is a unique identifier of NIC (Network Interface Controller). A network node can have multiple NIC but each with unique MAC.

####5.Connection-oriented and connection-less protocol
In connection-oriented protocol, acknowledgement is sent by the receiver. So it is reliable but slow. The example of connection-oriented protocol is TCP.  
But, in connection-less protocol, acknowledgement is not sent by the receiver. So it is not reliable but fast. The example of connection-less protocol is UDP.

####6.Socket
A socket is an endpoint between two way communication.


##Java Socket Programming 

Java Socket programming is used for communication between the applications running on different JRE.

Java Socket programming can be connection-oriented or connection-less.

Socket and ServerSocket classes are used for connection-oriented socket programming and DatagramSocket and DatagramPacket classes are used for connection-less socket programming.

The client in socket programming must know two information:

1. IP Address of Server,
2. Port number.



### Example of Java Socket Programming

```java
import java.io.*;
import java.net.*;
public class MyClient {
    public static void main(String[] args) {
        try{
            Socket s=new Socket("localhost",6666);
            DataOutputStream dout=new DataOutputStream(s.getOutputStr
            dout.writeUTF("Hello Server");
            dout.flush();//无需等到缓存区满，直接写入
            dout.close();
            s.close();
        }catch(Exception e){System.out.println(e);}
    }
}
                                                       
import java.io.*;
import java.net.*;
public class MyServer {
    public static void main(String[] args){
        try{
            ServerSocket ss=new ServerSocket(6666);
            Socket s=ss.accept();//establishes connection
            DataInputStream dis=new DataInputStream(s.getInputStream
            String  str=(String)dis.readUTF();
            System.out.println("message= "+str);
            ss.close();
        }catch(Exception e){System.out.println(e);}
    }
}
```

### Example of Java Socket Programming (Read-Write both side)

```java
import java.net.*;
import java.io.*;
class Server{
    public static void main(String args[])throws Exception{
        ServerSocket ss=new ServerSocket(3333);
        Socket s=ss.accept();
        DataInputStream din=new DataInputStream(s.getInputStream());
        DataOutputStream dout=new DataOutputStream(s.getOutputStream());
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));

    String str="",str2="";
    while(!str.equals("stop")){
        str=din.readUTF();
        System.out.println("client says: "+str);
        str2=br.readLine();
        dout.writeUTF(str2);
        dout.flush();
    }
    din.close();
    s.close();
    ss.close();
	}
}

import java.net.*;
import java.io.*;
class Server{
    public static void main(String args[])throws Exception{
        ServerSocket ss=new ServerSocket(3333);
        Socket s=ss.accept();
        DataInputStream din=new DataInputStream(s.getInputStream());
        DataOutputStream dout=new DataOutputStream(s.getOutputStream());
        BufferedReader br=new BufferedReader(new InputStreamReader(System.in));

        String str="",str2="";
        while(!str.equals("stop")){
            str=din.readUTF();
            System.out.println("client says: "+str);
            str2=br.readLine();
            dout.writeUTF(str2);
            dout.flush();
        }
        din.close();
        s.close();
        ss.close();
    }
}
```



## Java URL

The **Java URL** class represents an URL. URL is an acronym for **Uniform Resource Locator**. It points to a resource on the World Wide Web. For example:

http://www.javatpoint.com/java-tutorial

A URL contains many information:

1. **Protocol:** In this case, http is the protocol.
2. **Server name or IP Address:** In this case, www.javatpoint.com is the server name.
3. **Port Number:** It is an optional attribute. If we write http//ww.javatpoint.com:80/sonoojaiswal/ , 80 is the port number. If port number is not mentioned in the URL, it returns -1.
4. **File Name or directory name:** In this case, index.jsp is the file name.

### Example of Java URL Class

```java
//URLDemo.java
import java.io.*;
import java.net.*;
public class URLDemo{
    public static void main(String[] args){
        try{
            URL url=new URL("http://www.javatpoint.com/java-tutorial");

        	System.out.println("Protocol: "+url.getProtocol());
        	System.out.println("Host Name: "+url.getHost());
        	System.out.println("Port Number: "+url.getPort());
        	System.out.println("File Name: "+url.getFile());

    	}catch(Exception e){System.out.println(e);}
	}
}
```



## Java URLConnection Class

The **Java URLConnection** class represents a communication link between the URL and the application. This class can be used to read and write data to the specified resource referred by the URL.



### How to get the object of URLConnection

The openConnection() method of URL class returns the object of URLConnection class. Syntax:

```java
public URLConnection openConnection()throws IOException{}  
```



### Displaying source code of a webpage by URLConnecton class

The **URLConnection** class provides many methods, we can display all the data of a webpage by using the **getInputStream()** method. The getInputStream() method returns all the data of the specified URL in the stream that can be read and displayed.



### Example of Java URLConnection class

```java
import java.io.*;
import java.net.*;
public class URLConnectionExample {
    public static void main(String[] args){
        try{
            URL url=new URL("http://www.baidu.com");
            URLConnection urlcon=url.openConnection();
            InputStream stream=urlcon.getInputStream();
            int i;
            while((i=stream.read())!=-1){
                System.out.print(i);
            }
        }catch(Exception e){System.out.println(e);}
    }
}
```



## Java HttpURLConnection class

The **Java HttpURLConnection** class is http specific URLConnection. It works for HTTP protocol only.

By the help of HttpURLConnection class, you can information of any HTTP URL such as header information, status code, response code etc.

The java.net.HttpURLConnection is subclass of URLConnection class.

### How to get the object of HttpURLConnection class

The openConnection() method of URL class returns the object of URLConnection class. Syntax:

```java
public URLConnection openConnection()throws IOException{};
```

You can typecast it to HttpURLConnection type as given below.

```java
URL url=new URL("http://www.javatpoint.com/java-tutorial");    
HttpURLConnection huc=(HttpURLConnection)url.openConnection();  
```



### Java HttpURLConnecton Example

```java
import java.io.*;
import java.net.*;
public class HttpURLConnectionDemo{
    public static void main(String[] args){
        try{
            URL url=new URL("http://www.javatpoint.com/java-tutorial");
            HttpURLConnection huc=(HttpURLConnection)url.openConnection();
            for(int i=1;i<=8;i++){
                System.out.println(huc.getHeaderFieldKey(i)+" = "+huc.getHeaderField(i));
            }
            huc.disconnect();
        }catch(Exception e){System.out.println(e);}
    }
}
```



## Java InetAddress class

**Java InetAddress** class represents an IP address. The java.net.InetAddress class provides methods to get the IP of any host name *for example* www.javatpoint.com, www.google.com, www.facebook.com etc.

### Commonly used methods of InetAddress class

|                            Method                            |                         Description                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| public static InetAddress getByName(String host) throws UnknownHostException | it returns the instance of InetAddress containing LocalHost IP and name. |
| public static InetAddress getLocalHost() throws UnknownHostException | it returns the instance of InetAdddress containing local host name and address. |
|                 public String getHostName()                  |         it returns the host name of the IP address.          |
|                public String getHostAddress()                |         it returns the IP address in string format.          |

### Example of Java InetAddress class

```java
import java.io.*;
import java.net.*;
public class InetDemo{
    public static void main(String[] args){
        try{
            InetAddress ip = InetAddress.getByName("www.javatpoint.com");

        	System.out.println("Host Name: "+ip.getHostName());
        	System.out.println("IP Address: "+ip.getHostAddress());
   		}catch(Exception e){
            System.out.println(e);
        }
	}
}
```



## DatagramSocket class

Java DatagramSocket and DatagramPacket classes are used for **connection-less** socket programming.



### Example of Sending and Receiving  DatagramPacket by DatagramSocket

```java
//DSender.java
import java.net.*;
public class DSender{
    public static void main(String[] args) throws Exception {
        DatagramSocket ds = new DatagramSocket();
        String str = "Welcome java";
        InetAddress ip = InetAddress.getByName("127.0.0.1");

    DatagramPacket dp = new DatagramPacket(str.getBytes(), str.length(), ip, 3000);
    ds.send(dp);
    ds.close();
}

}

//DReceiver.java
import java.net.*;
public class DReceiver{
    public static void main(String[] args) throws Exception {
        DatagramSocket ds = new DatagramSocket(3000);
        byte[] buf = new byte[1024];
        DatagramPacket dp = new DatagramPacket(buf, 1024);
        ds.receive(dp);
        String str = new String(dp.getData(), 0, dp.getLength());
        System.out.println(str);
        ds.close();
    }
}
```



