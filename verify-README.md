# commands for verification

### verify checksums of rpm-gpg keys
```bash
#!/bin/bash

checksum="56ab59a81d07cd0206b557853baeb891a95471eb148660a8e5e4f2a9a03d9220"
for i in 


	inspector=$(sha256sum /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release)
	if [ $inspector != $checksum ]; then
           echo "ERROR: invalid checksum for rpm-gpg key"
           exit 1
        else
            echo "SUCCESS: checksum for rpm-gpg key is valid"
        fi

done


#command line example:

checksum="56ab59a81d07cd0206b557853baeb891a95471eb148660a8e5e4f2a9a03d9220"; inspector=$(sha256sum /etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release | cut -d " " -f 1); if [ $inspector != $checksum ]; then echo "ERROR: invalid checksum for rpm-gpg key"; else echo "SUCCESS: checksum for rpm-gpg key is valid"; fi
```


### verify package exists
```bash
rpm  —quiet -q cronie-noanacron; if [ $? = 1 ]; then echo "ERROR: cronie-noanacron is not installed on $(hostname)"; else echo "SUCCESS: cronie-noanacron is installed on $(hostname)"; fi
```

### verify docker container image
```bash
#!/bin/bash

container_image="registry/logstash:20190515"

for i in cef{1..4}; do
   echo "inspecting container $i"
   inspector=$(docker inspect -f '{{.Config.Image}}' $i)
   if [ $inspector != $container_image ]; then
     echo "ERROR: The $i container is running on the incorrect docker image."
     exit 1
   else
      echo "SUCCESS: The $i container is running on the correct docker image."
   fi
done
```

### verify auditd has correct setting
```bash
#!/bin/bash

SPACE="75"
INSPECTOR=$(grep -w space_left /etc/audit/auditd.conf | awk '{print $3}')

if [ $INSPECTOR != $SPACE ]; then
  echo -e "\n ERROR: space left is at wrong capacity \n Current capacity is $INSPECTOR. Should be $SPACE \n"
  exit 1
else
  echo -e "\n SUCCESS: space is at the correct capacity of $SPACE \n"
fi
```
