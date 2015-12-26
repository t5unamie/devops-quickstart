#devops-quickstart
example toolset used for the "The road to feeding myself using Devops" blog series. https://www.linkedin.com/pulse/road-feeding-myself-using-devops-step-1-how-what-why-johnathan-phan?trk=prof-post

# Notes

Looking to create seperate repo with the advanced automated and scripted setup for all tools being intergrated togeather. This one will have hand crafted configuration files insstead of template. Post 3 or 4 will teach ops guys what to automate.

Password for demo

username - example-admin
password - root

# Instructions

# Step 1 - Install docker on the OS of your choice

https://docs.docker.com/v1.8/installation/ubuntulinux/

# Step 2 - Setup Data directory for all your code.

mkdir -p  /data/nexus
mkdir –p /data/jenkins
chown -R 999:999 /data/nexus
chown -R 1000:1000 /data/jenkins

# Step 3 - Install proxy server – create file

sudo docker run --restart=always -p 80:80 -p 443:443 --name rp-server -v /data/nginx/conf.d:/etc/nginx/conf.d -d nginx

# Step 4 - Install openldap server

docker run --name central-auth --restart always -p 389:389 -v /data/ldap/db:/var/lib/ldap -v /data/ldap/config:/etc/ldap/slapd.d -v /data/ldap/ssl/:/etc/ldap/ssl -e LDAP_DOMAIN=some.domain.com -e LDAP_ORGANISATION=company -e LDAP_ROOTPASS=root -e LDAP_ADMIN_PWD=root -d osixia/openldap

docker run --name="ldap-admin" --restart=always -p 8888:443 -e PHPLDAPADMIN_LDAP_HOSTS="[{'172.17.0.1': [{'server': [{'tls': False}]},{'login': [{'bind_id': 'cn=admin,dc=some,dc=domain,dc=com'}]}]}, '172.17.0.1']" -d osixia/phpldapadmin

# Step 5 - Install Sonar

docker run -d --restart always -p 9000:9000 -v /data/sonar/conf:/opt/sonarqube/conf -v /data/sonar/logs:/opt/sonarqube/logs -v /data/sonar/plugins:/opt/sonarqube/extensions/plugins sonarqube

# Step 6 - Install Jenkins

docker run -p 8080:8080 -p 50000:50000 --name="jenkins" -v /data/jenkins:/var/jenkins_home -e JENKINS_OPTS="--prefix=/jenkins" -d jenkins



