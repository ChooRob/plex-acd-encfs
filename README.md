# plex-acd-encfs
Mount EncFS from Amazon Cloud Drive for Plex using Docker

TBU:
docker run -d --name plex-acd-encfs \
--cap-add SYS_ADMIN \
--device /dev/fuse \
--network=host \
--privileged \
--restart=always \
-e TZ="America/Toronto" \
-e ACD_CLI_CACHE_PATH="/root/.cache/acd_cli" \
-e ACD_ENCRYPTED_SUBDIR="/Share" \
-e MOUNT_UID="1000" \
-e MOUNT_GID="1000" \
-v /local/path/to/plex:/config \
-v /local/path/to/transcode:/transcode \
-v /local/path/to/.encfs6.xml:/etc/.encfs6.xml:ro \
-v/local/path/to/.EncFS:/etc/.ENCFS_PASSWORD:ro \
-v /local/path/to/.cache/acd_cli:/root/.cache/acd_cli \
-v /run \
robck/plex-acd-encfs

Required:

Plex configuration library path
-v /local/path/to/plex/library:/config \

Plex transcode folder location
-v /local/path/to/transcode:/transcode \

Environment variable for acd_cli cache path
-e ACD_CLI_CACHE_PATH="/root/.cache/acd_cli" \

Location in amazon cloud drive for Encrypted data
-e ACD_ENCRYPTED_SUBDIR="/Share" \

File containting password for decrypting EncFS:
-v /local/path/to/<Password file>:/etc/.ENCFS_PASSWORD:ro \

EncFS config (default target is /etc/.encfs6.xml, see below to change it):
-v /local/path/to/.encfs6.xml:/etc/.encfs6.xml:ro

Writable directory for acd_cli cache, must contain oauth_data:
-v /local/path/to/acd_cli:/root/.cache/acd_cli

Directory to mount the unencrypted files:
-v /local/mount/target:/mnt/encrypted:shared

Optional:
Location of the encrypted directory within the mounted Amazon Cloud Drive directory (leading forward slash required):
-e ACD_ENCRYPTED_SUBDIR="/"

Change the EncFS config location in the container:
-e ENCFS6_CONFIG="/etc/.encfs6.xml"

Change UID and GID of mounted files:
-e MOUNT_UID="1000" \
-e MOUNT_GID="1000"
