# File Exchange Linked In Public Environment

## Messages

Messages are composed by a 1-byte header (TYPE) and a message section which depends on the type of message.

[TYPE | MESSAGE]

### GETDIR 
Request to the target to send the paths of all the files being shared. Target should reply with at least one DIR message.

[] = EMPTY MESSAGE

### DIR
Reply to a GETDIR request.

[‘REMOTEPATH/0’]

REMOTEPATH (UTF-8 String): Path of the file in the sender. If it's empty, no more files are available.

### GETFILE
Request to the target to send the information for a file being shared.

[‘REMOTEPATH/0’]

REMOTEPATH (UTF-8 String): Path of the file in the target.

### FILE
Reply to a GETFILE request.

[FILEBYTES | SEQBYTES | FILEHASH | ‘REMOTEPATH/0’]

FILEBYTES (8 bytes): Total amount of bytes in the file.

SEQBYTES (2 bytes): Size of the sequence block in bytes.

FILEHASH (4 bytes): CRC-32 verification hash for the file.

REMOTEPATH (UTF-8 String): Path of the file in the sender.

### FNF
Reply to a GETFILE or GETBLK request if the requested file was not found.

[‘REMOTEPATH/0’]

REMOTEPATH (UTF-8 String): Path of the file in the sender.

### GETBLK
Request to the target to send the data for a block identified by a sequence number from a file being shared.

[SEQN | ‘REMOTEPATH/0’]

SEQN (4 bytes): Sequence number of the block to be retrieved.

REMOTEPATH (UTF-8 String): Path of the file in the target.

### BLK
Reply to a GETBLK request.

[BLK | SEQN | DATA | ‘REMOTEPATH/0’]

SEQN (4 bytes): Sequence number of the block.

DATA (SEQBYTES bytes): Raw data of the block. The size is fixed even if it's the last block of the file. The size of the block matches the one specified by the FILE message for the file in the path.

REMOTEPATH (UTF-8 String): Path of the file in the sender.

## Commands

### mount
*mount dir*

Adds specified directory to shared directories
  
### unmount
*unmount dir*

Removes directory from shared directories.

### getfile
*getfile interface address remotepath localpath*

Sends the request to download a file in the ‘remotepath’ to the ‘localpath’.

Starts downloading automatically as soon as the file information is returned.
  
### getdir
*getdir interface address*

Sends a request for all available files and their directories.
