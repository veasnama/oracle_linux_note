## Configuring an SSH Login without password

- SSH is ideal for managing remote systems because of its password-less option that uses keys instead of passwords, keeping system passwords safe. This article uses ssh-copy-id, a utility that greatly simplifies the procedure by copying the local host’s public key to the remote host’s authorized keys file and by verifying file permissions and ownership.

- The following procedure configures password-less SSH
---
1. Start by generating a key pair. A key pair includes a .pub (public key) that you share with remote computers and a private key that you never share.
---
ssh-keygen -t rsa
- Note: When you generate these keys, do not enter a passphrase:

```
# ssh-keygen -t rsa 
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): /root/.ssh/my_id 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /root/.ssh/my_id.
Your public key has been saved in /root/.ssh/my_id.pub.
The key fingerprint is:
1c:ee:bb:73:b2:42:34:02:e2:85:bf:c9:97:01:d1:f7 root@cae.netezza.com
The key's randomart image is:
+--[ RSA 2048]----+
|  ..o            |
|...o . .         |
|..o.. . o        |
| . ...oo E       |
|  . ooo.S        |
|   + o..         |
|    ..  .        |
|      . o..      |
|       .+*       |
+-----------------+

```
2. Navigate to the directory in which you created the keys and verify that the process succeeded. The following is a continuation of the example

```
# cd /root/.ssh/
```
3. Copy the public key to the destination system. That is, copy it to the system that you want password-less SSH access to 

```
# ssh-copy-id -i my_id.pub root@system-access
```
4. You should now be able to login into the remote machine without a password


# Troubleshooting

- If the public key is disabled, check the configuration file named /etc/ssh/sshd on the target computer for the following settings:

- RSAAuthentication yes
- PubkeyAuthentication yes


- If not already set, file permissions for ~.ssh need to be set to 700 as follows:
# cd
# chmod 700 .ssh


- If you are using an unsupported key type, generate a DSA key in Step 1 instead of an RSA key.
- ssh-keygen -t dsa
