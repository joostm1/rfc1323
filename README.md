# rfc1323
<h2>TCP window scale option</h2>
I recently stumbled on an issue that was blocking a project and that took some time and learning to solve it.
This short post to share my learning!

<h2>Issue</h2>
NFS client is AIX 7.1 located in an on premises DC and NFS server is Suse Linux 15 located in Azure.
<img src=https://github.com/joostm1/rfc1323/blob/master/export-migration.png alt="Initial setup"></img>

The issue here was that the throughput of the clients writing to the share was <b>disappointing slow</b> and <b>very fluctuating</b>.
Writing a 1GB file showed a speed anywhere between 1MB/s and at best around 20MB/s, and certainly never near the line capacity of 2Gb/s ~ 200MB/s. 
This throughput was an issue because we need to migrate and process lots of data over a weekend. No way we could make this with this throughput.

<h2>Troubleshooting</h2>
At the link level, there were no real signs of concern. Both server and client were connected at 1Gb/s full duplex link rate and there were no excessive drops on the adapters and switch ports.

I then created a packet capture at the NFS server and captured the copying of 1 file.