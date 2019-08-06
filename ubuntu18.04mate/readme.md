## Download Vagrant box manually

```
wget https://vagrantcloud.com/martijnsips/boxes/UbuntuMate1804LTS/versions/1.0.0/providers/virtualbox.box -O UbuntuMate1804.box
vagrant box add martijnsips/UbuntuMate1804LTS UbuntuMate1804.box
```

Output:

```
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'martijnsips/UbuntuMate1804LTS' (v0) for provider:
    box: Unpacking necessary files from: file://C:/Users/rodri/Desktop/UbuntuMate1804.box
    box:
==> box: Successfully added box 'martijnsips/UbuntuMate1804LTS' (v0) for 'virtualbox'!
```
