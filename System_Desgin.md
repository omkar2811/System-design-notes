# System Design of Popular Applications and Services


The following document consists of the system design and architecture of the highly scaled and efficiently managed systems which provide services to the millions of people in the form of applications and softwares to the people in  various continents all over the world. This study would help us understand  the underlying architecture and help us gain knowledge in the field of system design.

# 1. Netflix System Design

Netflix system design is one of the most impressive system design. Netflix caters to billions of people globally online video content in the form of movies,web-series,etc to stream without any interuptions and provide a great user experience to all the people.

**Some impressive Netflix statistics for 2017.**

- Netflix has more than 110 million subscribers and increasing each passing day.
- Netflix operates in more than 200 countries. 
- Netflix has nearly $3 billion in revenue per quarter.
- Netflix adds more than 5 million new subscribers per quarter.
- Netflix plays more than 1 billion hours of video each week. As a comparison, YouTube streams 1 billion hours of video every day while  Facebook streams 110 million hours of video every day.
- Netflix played 250 million hours of video on a single day in 2017.
- Netflix accounts for over 37% of peak internet traffic in the United States

Main System Design of Netflix
Netflix operates in two clouds: 

**AWS and Open Connect.**

Both clouds must work together seamlessly to deliver endless hours of customer-pleasing video.

The three parts of Netflix:


- **Client** 
- **Backend**
- **Content delivery network (CDN).**

he client is the user interface on any device used to browse and play Netflix videos. It could be an app on your iPhone, a website on your desktop computer, or even an app on your Smart TV. Netflix controls each and every client for each and every device. 

Everything that happens before you hit play happens in the backend, which runs in AWS. That includes things like preparing all new incoming video and handling requests from all apps, websites, TVs, and other devices.

Everything that happens after you hit play is handled by Open Connect. Open Connect is Netflix’s custom global content delivery network (CDN). Open Connect stores Netflix video in different locations throughout the world. When you press play the video streams from Open Connect, into your device, and is displayed by the client. Don’t worry; we’ll talk more about what a CDN is a little later.

- **USP of Netflix is Relaibility**
Netflix is so reliable now because they’ve taken extraordinary steps to make their service reliable. 

Netflix operates out of three AWS regions: one in North Virginia, one in Portland Oregon, and one in Dublin Ireland. Within each region, Netflix operates in three different availability zones.

Netflix has said there are no plans to operate out of more regions. It’s very expensive and complicated to add new regions. Most companies operate out of just one region, let alone two or three. 

The advantage of having three regions is that any one region can fail, and the other regions will step in handle all the members in the failed region. When a region fails, Netflix calls this evacuating a region.

**Netflix runs monthly tests. Every month Netflix causes a region to fail on purpose just to make sure its system can handle region level failures. A region can be evacuated in six minutes.**

# What Happens in AWS Before you Press Play?

Anything that doesn’t involve serving video is handled in AWS. 

This includes scalable computing, scalable storage, business logic, scalable distributed databases, big data processing and analytics, recommendations, transcoding, and hundreds of other functions. 

**Scalable computing and scalable storage.**

Scalable computing is EC2 and scalable storage is S3. Nothing new for us here. 

