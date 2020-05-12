# System Design of Popular Applications and Services


The following document consists of the system design and architecture of the highly scaled and efficiently managed systems which provide services to the millions of people in the form of applications and softwares to the people in  various continents all over the world. This study would help us understand  the underlying architecture and help us gain knowledge in the field of system design.

# 1. Netflix System Design

Netflix system design is one of the most impressive system design. Netflix caters to billions of people globally th online content to stream without interuptions in their service.

**Some impressive Netflix statistics for 2017.**

Netflix has more than 110 million subscribers and increasing each passing day.
Netflix operates in more than 200 countries. 
Netflix has nearly $3 billion in revenue per quarter.
Netflix adds more than 5 million new subscribers per quarter.
Netflix plays more than 1 billion hours of video each week. As a comparison, YouTube streams 1 billion hours of video every day while  Facebook streams 110 million hours of video every day.
Netflix played 250 million hours of video on a single day in 2017.
Netflix accounts for over 37% of peak internet traffic in the United States

Main System Design of Netflix
Netflix operates in two clouds: 

**AWS and Open Connect.

Both clouds must work together seamlessly to deliver endless hours of customer-pleasing video.

The three parts of Netflix:


**-client 
**-backend
**-content delivery network (CDN).

he client is the user interface on any device used to browse and play Netflix videos. It could be an app on your iPhone, a website on your desktop computer, or even an app on your Smart TV. Netflix controls each and every client for each and every device. 

Everything that happens before you hit play happens in the backend, which runs in AWS. That includes things like preparing all new incoming video and handling requests from all apps, websites, TVs, and other devices.

Everything that happens after you hit play is handled by Open Connect. Open Connect is Netflix’s custom global content delivery network (CDN). Open Connect stores Netflix video in different locations throughout the world. When you press play the video streams from Open Connect, into your device, and is displayed by the client. Don’t worry; we’ll talk more about what a CDN is a little later.

