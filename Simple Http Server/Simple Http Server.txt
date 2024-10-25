using System;
using System.Net;
using System.Text;

namespace SimpleHttpServer
{
    class Program
    {
        static void Main(string[] args)
        {
            string url = "http://localhost:8080/";
            using HttpListener listener = new HttpListener();
            listener.Prefixes.Add(url);
            listener.Start();
            Console.WriteLine($"Server started. Listening at {url}");

            while (true)
            {
                HttpListenerContext context = listener.GetContext();
                HttpListenerRequest request = context.Request;
                HttpListenerResponse response = context.Response;

                string responseString = "<html><body><h1>Hello, world!</h1><p>This is a simple C# HTTP server.</p></body></html>";
                byte[] buffer = Encoding.UTF8.GetBytes(responseString);

                response.ContentLength64 = buffer.Length;
                response.ContentType = "text/html";
                response.StatusCode = 200;

                using var output = response.OutputStream;
                output.Write(buffer, 0, buffer.Length);

                Console.WriteLine($"Request from {request.RemoteEndPoint} handled.");
            }
        }
    }
}
