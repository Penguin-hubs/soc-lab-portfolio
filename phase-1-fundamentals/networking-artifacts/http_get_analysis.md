Deliverable 2: Wireshark Capture of HTTP GET Request

Roadmap Target: Phase 1, Weeks 1–2
Objective: Capture and isolate an unencrypted HTTP GET request to demonstrate fundamental packet analysis and filtering skills.

1. Capture Methodology

The capture was performed using Wireshark running on a Linux VM. The primary method was to isolate all unencrypted web requests using a precise display filter.

Target Website: http://neverssl.com

Isolation Filter: http.request.method == "GET"

Local IP (Source): 10.0.2.15 (Your VM's address)

Foreign IP (Destination): 5.79.70.209 (The neverssl.com server)

2. Packet Flow and Context

The isolated packet (e.g., Packet 427 in the capture) is an Application Layer data exchange. It is critical to note that this request only happens after the Transport Layer establishes a connection.

The client and server first performed the TCP 3-Way Handshake ($\text{SYN}$, $\text{SYN-ACK}$, $\text{ACK}$) on Destination Port 80. This established a reliable tunnel, after which the client was able to send the actual $\text{HTTP}$ application data (the $\text{GET}$ request).

3. Analysis of Key Evidence

By expanding the Hypertext Transfer Protocol section in the packet details, the following critical evidence was observed:

Transport Layer Confirmation: The traffic utilized Destination Port 80, confirming it as unencrypted $\text{HTTP}$ communication.

The Request Command: The exact command sent by the browser was GET / HTTP/1.1. This is the fundamental request to retrieve the index page (/) of the web server.

Target Host: The $\text{HTTP}$ header confirmed the request was intended for the Host: neverssl.com server.

Client Information: The User-Agent field identified the client software used to make the request (e.g., Firefox/86.0).

4. Conclusion

This capture successfully demonstrates the ability to differentiate unencrypted $\text{HTTP}$ traffic, apply precise $\text{Wireshark}$ Display Filters (http.request.method), and identify the key components of an $\text{Application}$ $\text{Layer}$ request. This skill is foundational for analyzing web-based threats as a SOC Analyst.
