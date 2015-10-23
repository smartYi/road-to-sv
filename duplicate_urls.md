# Duplicate URLs

You have 10 billion URLs. How do you detect eh duplicate documents? In this case, assume "duplicate" means that the URLs are identical

## Solution

If each URL is an average of 100 characters, and each character is 4 bytes, then this list of 10 billion URLs will take up about 4 terabytes.

Can't put everything in the memory.

split the data to chunks

multiple machines: parallel, sync problem