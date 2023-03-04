# ovpn
openvpn install and radius


===================install script ==================


curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
chmod +x openvpn-install.sh
./openvpn-install.sh


==================================radius=============
###################On CentOS we run:


yum install libgcrypt libgcrypt-devel gcc-c++ lsof psmisc -y
#####################On Ubuntu we run:



apt-get install libgcrypt20 libgcrypt20-dev gcc make build-essential


#################Now we need to grab the Radius Plugin:


wget https://vpnextra.com/download/radiusplugin_v2.1a_beta1.tar.gz


###################Decompress it:


tar xvfz radiusplugin_v2.1a_beta1.tar.gz


###################Move to its directory:


cd radiusplugin_v2.1a_beta1/


####################Compile it:

make


#######################The output will be a single radiusplugin.so file.  Now move the file and the .cnf file to the OpenVPN directory using the below commands:


cp radiusplugin.so /etc/openvpn/
cp radiusplugin.cnf /etc/openvpn/

####################First thing, edit the radiusplugin.cnf file and focus on the server section and ensure that the details are correct:


server
{
        # The UDP port for radius accounting.
        acctport=1813
        # The UDP port for radius authentication.
        authport=1812
        # The name or ip address of the radius server.
        name=YOUR RADIUS SERVER IP
        # How many times should the plugin send the if there is no response?
        retry=1
        # How long should the plugin wait for a response?
        wait=1
        # The shared secret.
        sharedsecret=YOUR RADIUS SERVER SECRET
}


################################Now edit your OpenVPN config file and remove any other plugin lines, then add the following:


plugin /etc/openvpn/radiusplugin.so /etc/openvpn/radiusplugin.cnf


service openvpn restart
