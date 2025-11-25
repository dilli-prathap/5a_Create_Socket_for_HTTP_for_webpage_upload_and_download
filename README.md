# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 

```python
import socket


def send_request(host, port, request):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request.encode())
        response = s.recv(4096).decode()
    return response


def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()
        content_length = len(file_data)

        request = (
            f"POST /upload HTTP/1.1\r\n"
            f"Host: {host}\r\n"
            f"Content-Length: {content_length}\r\n"
            f"Content-Type: text/plain\r\n"
            f"\r\n"
        )
        request = request + file_data.decode(errors='ignore')
    
    response = send_request(host, port, request)
    return response


def download_file(host, port, filename):
    request = f"GET /{filename} HTTP/1.1\r\nHost: {host}\r\n\r\n"
    response = send_request(host, port, request)

    parts = response.split('\r\n\r\n', 1)
    if len(parts) > 1:
        file_content = parts[1]
        with open('downloaded_' + filename, 'wb') as file:
            file.write(file_content.encode())
        print(f"{filename} downloaded successfully.")
    else:
        print("Error: No file content found in response.")


if __name__ == "__main__":
    host = 'example.com'   # Replace with a real server or localhost
    port = 80              # HTTP default port

    # Upload file
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:", upload_response)

    # Download file
    download_file(host, port, 'example.txt')
```
## OUTPUT
<img width="1009" height="498" alt="516766164-92100548-c4bf-4aba-9f05-4535011b09a9" src="https://github.com/user-attachments/assets/4801dc79-2f78-4024-b7d3-054132be9984" />


## Result
Thus the socket for HTTP for web page upload and download created and Executed
