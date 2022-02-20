# URL Shortening service
It is a back end service written in go, which let's users shorten a long url link, by replacing it with a MD5 encrypted version of the link text.

# Design requirements
- Usersshould be able to register a link and create an alias for it. They should also be able to specify how long does the shortened url shold work. This should default to five years of free tier retention period.
- input traffic for redirection is 20,000 per second.

# Deductions from the design requirements
- It is a read heavy service. R:W::100:1.
- Assuming the input and output is both limited to 500 bytes.
- Capacity estimations :
    - 200 new url signups per second translate to 31536000000 urls stored at saturation (5 years) assuming max retention period for each.
    - This translates to about 15768000000 bytes ~16 GB.
    - 1:1 mapping between shortened and original link points towards the need for a key value data storage.
    - Some websites might make up most of the traffic, hence we way use some sort of caching

# Service Architecture
- Refer docs/service-architecture.io