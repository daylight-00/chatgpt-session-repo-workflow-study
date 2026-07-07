# Archive note

The intended archival workflow is:

1. preserve the report directory and file modification timestamps;
2. create a `.tar.gz` containing the full top-level directory;
3. attach the archive to a GitHub Release.

In this session, the archive can be created locally with timestamps stored in the tar metadata. The available GitHub connector surface did not expose Release creation or Release asset upload, and the shell did not have GitHub CLI credentials. The study repository itself was created manually and then became accessible to the connector. Release publication is intentionally deferred for this project stage. The current connector surface still does not expose Release creation or asset upload.
