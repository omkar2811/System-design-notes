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

**USP of Netflix is Relaibility.**

Netflix is so reliable now because they’ve taken extraordinary steps to make their service reliable. 

Netflix operates out of three AWS regions: one in North Virginia, one in Portland Oregon, and one in Dublin Ireland. Within each region, Netflix operates in three different availability zones.

Netflix has said there are no plans to operate out of more regions. It’s very expensive and complicated to add new regions. Most companies operate out of just one region, let alone two or three. 

The advantage of having three regions is that any one region can fail, and the other regions will step in handle all the members in the failed region. When a region fails, Netflix calls this evacuating a region.

**Netflix runs monthly tests. Every month Netflix causes a region to fail on purpose just to make sure its system can handle region level failures. A region can be evacuated in six minutes.**

# What Happens in AWS Before you Press Play?

Anything that doesn’t involve serving video is handled in AWS. 

This includes scalable computing, scalable storage, business logic, scalable distributed databases, big data processing and analytics, recommendations, transcoding, and hundreds of other functions. 

**Scalable computing and scalable storage.**

Scalable computing is EC2 and scalable storage is S3.

**Scalable distributed database.** 

Netflix uses both DynamoDB and Cassandra for their distributed databases.

**Big data processing and analytics.**

Big data simply means there’s a lot of data. Netflix collects a lot of information. Netflix knows what everyone has watched when they watched it and where they were when they watched. Netflix knows which videos members have looked at but decided not to watch. Netflix knows how many times each video has been watched…and a lot more. 

**Netflix personalizes artwork just for you.**

When browsing around looking for something to watch on Netflix, have you noticed there’s always an image displayed for each video? That’s called the header image.

The header image is meant to intrigue you, to draw you into selecting a video. The idea is the more compelling the header image, the more likely you are to watch a video. And the more videos you watch, the less likely you are to unsubscribe from Netflix.

Let’s say one of your recommendations is the movie Good Will Hunting. Netflix must choose a header image to show you. The goal is to show an image that lets you know about a movie you’ll probably be interested in. Which image should Netflix show you? 

If you like comedies, Netflix will show you an image featuring Robin Williams. If you prefer romantic movies, Netflix will show you an image Matt Damon and Minnie Driver poised for a kiss.

**Recommendations.**

That’s part of the big data processing and analytics we just talked about. Netflix looks at its data and predicts what you’ll like. In fact, everything you see see on a Netflix screen was chosen specifically for you using machine learning.

# 2. URL Shortner

URL shortening is used to create shorter aliases for long URLs. 
URL shortening is used for optimizing links across devices, tracking individual links to analyze audience and campaign performance, and hiding affiliated original URLs.(Eg : tinyurl.com)

**Capacity Estimation and Constraints**

Traffic Estimates : 
  Assuming, we will have 500M new URL shortenings per month, with 100:1 read/write ratio.
  New URLs shortenings per second = 500M/(30 * 24 * 60 *60) = 200URLs/sec.
  Redirections / sec = 100 * 200 URLs/sec = 20K/sec
Storage Estimates :
  Assuming 5 years span .
  Total objects to store = 500M * 5yr * 12months = 30B.
  Assuming 500 bytes per object
  Total Storage = 30B * 500 = 15TB
Bandwidth Estimates : 
  For write requests : 200 * 500 = 100KB/s
  For read requests : 20K * 500 = 10MB/s
Memory Estimates :
  Will follow 20-20 rule.
  Requests per day = 20K * 60 * 60 *24 = 1.7B
  To Cache 20% of these we need 0.2 * 1.7B * 500 = 170GB.

**System API's**
createURL(api_dev_key, original_url, custom_alias=None, user_name=None, expire_date=None)
deleteURL(api_dev_key, url_key)
To prevent abuse :  Each api_dev_key can be limited to a certain number of URL creations and redirections per some time period.

**Database Schema**
URL :  Hash,OriginalURL,CreationDate,ExpirationDate,UserID
User : UserID,Name,...

**Basic System Design and Algorithm**

1.Encoding Actual URL : 
  In this we will compute an unique hash of the url and then encoded for displaying.
  If Base64 encoding is used with 6 letter key,we will have 64^7=68.7B unique .This will suffice.
  But the problems with this approach are :
    1. If multiple users enter the same URL, they can get the same shortened URL, which is not acceptable.
    2. What if parts of the URL are URL-encoded?

2.Generating keys offline : 
  We can have a standalone Key Generation Service (KGS) that generates random six-letter strings beforehand and stores them in a database (let’s call it key-DB). Whenever we want to shorten a URL, we will just take one of the already-generated keys and use it.
  Can concurrency cause problems?Servers can use KGS to read/mark keys in the database. KGS can use two tables to store keys: one for keys that are not used yet, and one for all the used keys. As soon as KGS gives keys to one of the servers, it can move them to the used keys table. KGS can always keep some keys in memory so that it can quickly provide them whenever a server needs them.
  we can have a standby replica of KGS to avoid SPF.
  
**Data Partitioning and Replication**
  We can use consistent hashing.


We can add a Load balancing layer at three places in our system:

Between Clients and Application servers
Between Application Servers and database servers
Between Application Servers and Cache servers


A separate Cleanup service can run periodically to remove expired links from our storage and cache. This service should be very lightweight and can be scheduled to run only when the user traffic is expected to be low.
