## Custom hostnames based on DNS

It's is possible to have different hostnames for cluster members other than the default configured by the service providers.
When the cluster member machines spin up they try to make a reverse DNS lookup and if it returns a valid value, then it is set as hostname.

### Configure reverse DNS on AWS

On AWS you have two options:

* use Route53 as DNS provider
* setup your own DNS server in the VPC

Both setup requires to have a VPC and attach custom DHCP option to it.

#### Using Route53


1. **Create or have a VPC**
    1. CIDR eg. *10.1.0.0/16*
    2. Enable DNS resolution and **DNS hostnames**
    3. Create a subnet with CIDR eg. *10.1.1.0/28*
    > Notice that you might want to set an Internet Gateway for the VPC and add a default route to the routing table for the IGW.
    Also you may enable *Auto-assign Public IP* option.
    This way Cloudbreak would reach the cluster from outside of the VPC and the cluster would have internet access. 
2. **Setup DHCP options**
    1. Set the domain name to a preferred domain, eg. cloudbreak.beer
    2. Set domain name servers to AmazonProviderDNS
    
        <a href="../images/aws-route53-dhcp-option.png" target="_blank" title="click to enlarge"><img src="../images/aws-route53-dhcp-option.png" width="350" title="DHCP option for Route53"></a>   
    3. On the VPC side use *Edit DHCP Options Set* to use the created DHCP option for the VPC

3. **Configure domain at Route53**
    1. Create a Hosted Zone
        * with the name used previously with DHCP Options
        * set the type to private
        * select the VPC you assigned the DHCP option to
    2. Add records
        * Go to *Record set*
        * Use *Create Record Set* for every available IP to have a custom name (10.1.1.4-14 if you have the subnet used above)
            * Type: A
            * Name: eg. *b10*
            * Value: eg. *10.1.1.10*
            
                <a href="../images/aws-route53-record.png" target="_blank" title="click to enlarge"><img src="../images/aws-route53-record.png" width="350" title="Domain record for Route53"></a>
    
    3. Create another hosted zone for reverse DNS lookup. 
        * It's domain name should look like this if you used the previous subnet:
        
            <pre>1.1.10.in-addr.arpa.</pre>
        
        * attach to VPC
        * type should be private

    4. Add records (for every created domain)
        * Type: PTR
        * Name: is the IP last part eg. *10*
        * Value: the domain name you set in the previous step eg. *b10*
        
            <a href="../images/aws-route53-reverse-record.png" target="_blank" title="click to enlarge"><img src="../images/aws-route53-reverse-record.png" width="350" title="Domain PTR record for Route53"></a>
            
4. **Create the cluster** in the VPC and you will have the hostnames same as the domain names.
> Notice that you don't have control the order over the IP address leased to the machines, so the names may not be in order.

#### Using custom DNS server


1. **Create or have a VPC**
    1. CIDR eg. *10.3.0.0/16*
    2. Enable DNS resolution and **DNS hostnames**
    3. Create a subnet with CIDR eg. *10.3.3.0/28*
    > Notice that you might want to set an Internet Gateway for the VPC and add a default route to the routing table for the IGW.
    Also you may enable *Auto-assign Public IP* option.
    This way Cloudbreak would reach the cluster from outside of the VPC and the cluster would have internet access.
2. **Setup DNS server** in your VPC/subnet
    1. In the configuration be sure to have DNS records and reverse DNS pointers for all IP address (eg. 10.3.3.4-14)
    2. Example unbound configuration:
    
        <pre>
            [root@ip-10-3-3-9 conf.d]# cat 00-cloudbreak.cloud.conf
            server:
                local-zone: "cloudbreak.cloud." static
                local-data: "aww1.cloudbreak.cloud. IN A 10.3.3.4"
                local-data-ptr: "10.3.3.4 aww1.cloudbreak.cloud."
                local-data: "aww2.cloudbreak.cloud. IN A 10.3.3.5"
                local-data-ptr: "10.3.3.5 aww2.cloudbreak.cloud."
                local-data: "aww3.cloudbreak.cloud. IN A 10.3.3.6"
                local-data-ptr: "10.3.3.6 aww3.cloudbreak.cloud."
                local-data: "aww4.cloudbreak.cloud. IN A 10.3.3.7"
                local-data-ptr: "10.3.3.7 aww4.cloudbreak.cloud."
                local-data: "aww5.cloudbreak.cloud. IN A 10.3.3.8"
                local-data-ptr: "10.3.3.8 aww5.cloudbreak.cloud."
                local-data: "aww6.cloudbreak.cloud. IN A 10.3.3.9"
                local-data-ptr: "10.3.3.9 aww6.cloudbreak.cloud."
                local-data: "aww7.cloudbreak.cloud. IN A 10.3.3.10"
                local-data-ptr: "10.3.3.10 aww7.cloudbreak.cloud."
                local-data: "aww8.cloudbreak.cloud. IN A 10.3.3.11"
                local-data-ptr: "10.3.3.11 aww8.cloudbreak.cloud."
                local-data: "aww9.cloudbreak.cloud. IN A 10.3.3.12"
                local-data-ptr: "10.3.3.12 aww9.cloudbreak.cloud."
                local-data: "aww10.cloudbreak.cloud. IN A 10.3.3.13"
                local-data-ptr: "10.3.3.13 aww10.cloudbreak.cloud."
                local-data: "aww11.cloudbreak.cloud. IN A 10.3.3.14"
                local-data-ptr: "10.3.3.14 aww11.cloudbreak.cloud."
        </pre>

3. **Setup DHCP options**
    1. Set the domain name to a preferred domain, eg. cloudbreak.cloud
    2. Set domain name servers to the previously created DNS server
    
        <a href="../images/aws-custondns-dhcp-option.png" target="_blank" title="click to enlarge"><img src="../images/aws-custondns-dhcp-option.png" width="350" title="DHCP option for own DNS server"></a>   
    3. On the VPC side use *Edit DHCP Options Set* to use the created DHCP option for the VPC

4. **Create the cluster** in the VPC and you will have the hostnames same as the domain names.
> Notice that you don't have control the order over the IP address leased to the machines, so the names may not be in order.