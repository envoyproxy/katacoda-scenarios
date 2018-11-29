With EDS in place it's possible to scale up the upstream clusters. If we wanted to be able to dynamaically add new domains and clusters, the cluster discovery service (CDS) API needs to be implemented.

<pre class="file" data-filename="cds.conf" data-target="replace">

</pre>

Based on how Docker handles fle inode tracking, sometimes the inotify filesystem change isn't triggered and detected. Force the change with the command `mv cds.conf tmp; mv tmp cds.conf`{{execute}}
