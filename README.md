zillow-cloudflare-testcase
==========================

zillow-cloudflare-testcase repository converges all API programs, documentations, test cases on/regarding CLOUDFARE application tests



Current name servers
Change your nameservers to
NS1.P27.DYNECT.NET
kip.ns.cloudflare.com
NS2.P27.DYNECT.NET
sue.ns.cloudflare.com
NS3.P27.DYNECT.NET
Delete this name server
NS4.P27.DYNECT.NET
Delete this name server

***********************
API Key for CLOUDFLARE     e8600c1378e8a74dc01bff6f185abc32c78fb

Client Interface API documentation

Regenerate

© CloudFlare, Inc.
What we do

    Plans
    Overview
    Features tour
    CloudFlare's network
    CloudFlare apps

Community

    Case studies
    CloudFlare blog
    Web badges
    Hosting partners
    Developers

Support

    Help center
    System status
    Downloads
    Trust & Safety

About us

    Our team
    Careers
    Press center
    Terms of service
    Privacy & security

*****************************

A
zillow.com
points to 192.211.12.20
Automatic
A
api
points to 192.211.12.30
Automatic
A
content
points to 192.211.12.29
Automatic
A
engineering
points to 184.73.191.37
Automatic
A
ftp
points to 192.211.12.44
Automatic
collapse
We added a FTP subdomain for you that does not pass through the CloudFlare network. If in the past you FTPed directly to zillow.com, now you should FTP to ftp.zillow.com. You can change the default name of the subdomain to something other than ftp for enhanced security.
A
image
points to 192.211.12.60
Automatic
A
mail
points to 206.132.3.45
Automatic
A
mobile
points to 192.211.12.35
Automatic
A
mx1
points to 192.211.12.46
Automatic
A
mx2
points to 192.211.12.46
Automatic
A
ops2
points to 192.211.12.45
Automatic
A
ops
points to 192.211.12.45
Automatic
A
pager
points to 192.211.12.45
Automatic
A
photos
points to 192.211.12.50
Automatic
A
sales
points to 192.211.12.49
Automatic
A
sitemap
points to 192.211.12.34
Automatic
A
splunk
points to 192.211.12.47
Automatic
A
www
points to 192.211.12.20
Automatic
A
zm
points to 192.211.12.25
Automatic
CNAME
blog
is an alias of sitemap.zillow.com
Automatic
CNAME
go
is an alias of mkto-sj010052.com
Automatic
CNAME
images
is an alias of image.zillow.com
Automatic
CNAME
img
is an alias of image.zillow.com
Automatic
CNAME
info
is an alias of zillow.mktoweb.com
Automatic
CNAME
investors
is an alias of abea-6aa1ju.client.shareholder.com
Automatic
CNAME
links
is an alias of sitemap.zillow.com
Automatic
CNAME
link
is an alias of sitemap.zillow.com
Automatic
CNAME
m
is an alias of mobile.zillow.com
Automatic
CNAME
partners
is an alias of sitemap.zillow.com
Automatic
CNAME
pictures
is an alias of picture.zillow.com
Automatic
CNAME
u
is an alias of sitemap.zillow.com
Automatic
CNAME
videos
is an alias of video.zillow.com
Automatic
MX
mail
mail handled by arm.bigfootinteractive.comwith priority 10
Automatic
MX
zillow.com
mail handled by ms89664146.msv1.invalidwith priority 100
Automatic
MX
zillow.com
mail handled by zillow-com.mail.protection.outlook.comwith priority 0
Automatic
TXT
zillow.com
v=spf1 include:spf.protection.outlook.com include:mktomail.com a:zillow.com ip4:96.43.144.64 ip4:96.43.144.65 ip4:96.43.148.64 ip4:96.43.148.65 ip4:182.50.78.0/25 ip4:204.14.232.0/25 ip4:204.14.234.0/25 ~all
Automatic


********************

1 - Overview

This document explains in detail the semantics of the function calls you can make using the Client Interface API.

In this document, you will learn:

    Retrieve basic statistics for your website
    Retrieve/Modify common CloudFlare settings
    Toggle Development Mode on/off
    Purge the CloudFlare cache and the Preloader Cache

This document is subject to change. The latest version of this document is always maintained at:

https://www.cloudflare.com/docs/client-api.html

If you have comments, find errors, or have questions, please contact help@cloudflare.com
1.1 - Interaction with the CloudFlare System

Interaction with the CloudFlare system is accomplished with POST requests through the secure HTTP protocol (HTTPS). This protocol was chosen for its simplicity, network administrators' familiarity with it, its ability to pass through many corporate firewalls without requiring their modification, the protocol's extensive documentation, and the wide availability of access tools written in many languages.

Some links concerning establishing and performing POST requests over secure HTTP sessions are provided below:

    Perl (LWP)
    Java (HttpsURLConnection)
    PHP (fsockopen)
    PHP (curl)
    Python (urllib)
    Windows (WinInet) | Invoke-WebRequest

1.2 - General Issues Checklist

There are some general issues that you should keep in mind when designing an application for interacting with this API. You may want to refer to this checklist as you are constructing the application. Neglecting to follow these general guidelines is the usual source of difficulty for application designers.

    All requests should be established via a secure SSL connection through the HTTPS protocol.
    All operation requests are initiated via a GET/POST request from the client to the CloudFlare system.
    The order in which you pass the parameters does not matter.
    All responses are returned as JSON.
    The precise order of the fields in the JSON responses, as described in the examples in this document, may change and should not be relied upon.
    Samples POSTS provided on this page use the common unix cURL utility. The -d options sets a POST parameter.
    All calls are rate-limited to 1200 every 5 minutes, unless otherwise specified.
    Ask for a JSONP callback by appending a &callback=mycallback parameter.

2 - Client Operations

All GET/POST requests should be directed at the client gateway interface, located at:
https://www.cloudflare.com/api_json.html

2.1 - Basic Parameters

Every GET/POST request must include at the following basic parameter(s):

    "tkn"

    This is the API key made available on your Account page.
    "email"

    The e-mail address associated with the API key.
    "a"

    To define which request is being made, the client should POST an "a" parameter. The "a" specifies which action you'd like to perform. Specific actions are described in Section 3 below.

2.2 - Error Conditions

If there are errors, the JSON response will include a detailed message in the "msg" field, and sometimes will include one of the following error codes:

    "E_UNAUTH" -- Authentication could not be completed
    "E_INVLDINPUT" -- Some other input was not valid
    "E_MAXAPI" -- You have exceeded your allowed number of API calls.

3 - Access
3.1 - "stats" - Retrieve domain statistics for a given time frame

Retrieve the current stats and settings for a particular website. This function can be used to get currently settings of values such as the security level.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition, you must pass the following parameters:

    "z"

    The zone (domain) that statistics are being retrieved from
    "interval"

    The time interval for the statistics denoted by these values:
        For these values, the latest data is from one day ago
        20 = Past 30 days
        30 = Past 7 days
        40 = Past day

        The values are for Pro accounts
        100 = 24 hours ago
        110 = 12 hours ago
        120 = 6 hours ago

Here is an example POST to the stats action for retrieving the past 7 days of statistics for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=stats' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'interval=20'


Output: 	


{

    -
    "response": {
        -
        "result": {
            "timeZero": 1333051372000,
            "timeEnd": 1335643372000,
            "count": 1,
            "has_more": false,
            -
            "objs": [
                -
                {
                    "cachedServerTime": 1335816172000,
                    "cachedExpryTime": 1335859372000,
                    -
                    "trafficBreakdown": {
                        -
                        "pageviews": {
                            "regular": 2640,
                            "threat": 27,
                            "crawler": 4
                        },
                        -
                        "uniques": {
                            "regular": 223,
                            "threat": 16,
                            "crawler": 4
                        }
                    },
                    -
                    "bandwidthServed": {
                        "cloudflare": 78278.706054688,
                        "user": 58909.374023438
                    },
                    -
                    "requestsServed": {
                        "cloudflare": 4173,
                        "user": 3697
                    },
                    "pro_zone": false,
                    "pageLoadTime": null,
                    "currentServerTime": 1335824051000,
                    "interval": 20,
                    "zoneCDate": 1307574643000,
                    "userSecuritySetting": "Medium",
                    "dev_mode": 0,
                    "ipv46": 5,
                    "ob": 0,
                    "cache_lvl": "agg"
                }
            ]
        }
    },
    "result": "success",
    "msg": null

}

3.2 - "zone_load_multi" - Retrieve the list of domains

This lists all domains in a CloudFlare account along with other data.
Input: 	

Requires the basic parameters described in Section 2.1 of this document.

Here is an example POST to the zone_load_multi action for retrieving the list of domains:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=zone_load_multi' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \


Output: 	


{

    -
    "request": {
        "act": "zone_load_multi",
        "a": "zone_load_multi",
        "email": "sample@example.com",
        "tkn": "8afbe6dea02407989af4dd4c97bb6e25"
    },
    -
    "response": {
        -
        "zones": {
            "has_more": false,
            "count": 3,
            -
            "objs": [
                -
                {
                    "zone_id": "42",
                    "user_id": "1",
                    "zone_name": "example-domain-1.com",
                    "display_name": "example-domain-1.com",
                    "zone_status": "V",
                    "zone_mode": "1",
                    "host_id": "415",
                    "zone_type": "P",
                    "host_pubname": "WebHost",
                    "host_website": "http://WEBSITEOFHOST.com",
                    "vtxt": null,
                    "fqdns": null,
                    "step": "4",
                    "zone_status_class": "status-active",
                    "zone_status_desc": "CloudFlare powered, this website will be accelerated and protected (<a class=\"modal-link-faq muted\" href=\"#\" onClick=\"cloudFlare.faq('en_US', ['CompleteActive']);return false;\">info</a>)",
                    "ns_vanity_map": [ ],
                    "orig_registrar": null,
                    "orig_dnshost": null,
                    "orig_ns_names": null,
                    -
                    "props": {
                        "dns_cname": 0,
                        "dns_partner": 1,
                        "dns_anon_partner": 0,
                        "pro": 0,
                        "expired_pro": 0,
                        "pro_sub": 0,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expired_rs_pro": 0,
                        "reseller_pro": 0,
                        "force_interal": 0,
                        "ssl_needed": 0,
                        "alexa_rank": 0
                    },
                    -
                    "confirm_code": {
                        "zone_deactivate": "f91eb75acb516aa95dabf5fa2b7305ec",
                        "zone_dev_mode1": "84fae8e497487d5dea45a39f00054488"
                    },
                    -
                    "allow": [
                        "analytics",
                        "threat_control",
                        "cf_apps",
                        "dns_editor",
                        "cf_settings",
                        "page_rules",
                        "zone_deactivate",
                        "zone_dev_mode1"
                    ]
                },
                -
                {
                    "zone_id": "43",
                    "user_id": "1",
                    "zone_name": "example-domain-2.net",
                    "display_name": "example-domain-2.net",
                    "zone_status": "V",
                    "zone_mode": "0",
                    "host_id": null,
                    "zone_type": "F",
                    "host_pubname": null,
                    "host_website": null,
                    "vtxt": null,
                    -
                    "fqdns": [
                        "norm.ns.cloudflare.com",
                        "pat.ns.cloudflare.com"
                    ],
                    "step": "4",
                    "zone_status_class": "status-deactivated",
                    "zone_status_desc": "Deactivated: Web traffic to this website will no longer pass through the CloudFlare system. We will continue to resolve your DNS. However, you will not receive the benefits of CloudFlare. You may <a href=\"/my-websites.html?act=zone_reactivate&z=example-domain-2.net\">reactivate</a> CloudFlare for this website at any time. (<a class=\"modal-link-faq muted\" href=\"javascript:void(0);\" onClick=\"cloudFlare.faq('en_US', ['DeactivateInProgress']);return false;\">help</a>)",
                    "ns_vanity_map": [ ],
                    "orig_registrar": "A Registrar!",
                    "orig_dnshost": null,
                    "orig_ns_names": "{NS1.OLDNAMESERVER.COM,NS2.OLDNAMESERVER.COM}",
                    -
                    "props": {
                        "dns_cname": 0,
                        "dns_partner": 0,
                        "dns_anon_partner": 0,
                        "pro": 0,
                        "expired_pro": 0,
                        "pro_sub": 0,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expired_rs_pro": 0,
                        "reseller_pro": 0,
                        "force_interal": 0,
                        "ssl_needed": 0,
                        "alexa_rank": 0
                    },
                    -
                    "confirm_code": {
                        "zone_delete": "79fece02a0b6db49ba25d78490142ba2"
                    },
                    -
                    "allow": [
                        "zone_reactivate",
                        "analytics",
                        "zone_delete",
                        "dns_editor",
                        "cf_settings",
                        "page_rules"
                    ]
                },
                -
                {
                    "zone_id": "44",
                    "user_id": "1",
                    "zone_name": "example-domain-3.org",
                    "display_name": "example-domain-3.org",
                    "zone_status": "V",
                    "zone_mode": "1",
                    "host_id": "115",
                    "zone_type": "P",
                    "host_pubname": "Some Host!",
                    "host_website": "WEBSITEOFHOST.org",
                    "vtxt": null,
                    "fqdns": null,
                    "step": "4",
                    "zone_status_class": "status-active",
                    "zone_status_desc": "CloudFlare powered, this website will be accelerated and protected (<a class=\"modal-link-faq muted\" href=\"#\" onClick=\"cloudFlare.faq('en_US', ['CompleteActive']);return false;\">info</a>)",
                    "ns_vanity_map": [ ],
                    "orig_registrar": null,
                    "orig_dnshost": null,
                    "orig_ns_names": null,
                    -
                    "props": {
                        "dns_cname": 0,
                        "dns_partner": 1,
                        "dns_anon_partner": 0,
                        "pro": 0,
                        "expired_pro": 0,
                        "pro_sub": 0,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expired_rs_pro": 0,
                        "reseller_pro": 0,
                        "force_interal": 0,
                        "ssl_needed": 0,
                        "alexa_rank": 0
                    },
                    -
                    "confirm_code": {
                        "zone_deactivate": "c85831462b7bf79d69ad82aba1fa02ce",
                        "zone_dev_mode1": "fc6cacf3b4ef5bbdb9c0099a9762ae94"
                    },
                    -
                    "allow": [
                        "analytics",
                        "threat_control",
                        "cf_apps",
                        "dns_editor",
                        "cf_settings",
                        "page_rules",
                        "zone_deactivate",
                        "zone_dev_mode1"
                    ]
                }
            ]
        }
    },
    "result": "success",
    "msg": null

}

3.3 - "rec_load_all" - Retrieve DNS Records of a given domain

Lists all of the DNS records from a particular domain in a CloudFlare account
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The domain that records are being retrieved from
    "o"

    Optional. If the has_more parameter in the JSON response is true, you can use o=N to offset the starting position for the response. By default, this call will list the first 180 records for a zone.

Here is an example POST to the rec_load_all action for retrieving the DNS records for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=rec_load_all' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com'


Output: 	


{

    -
    "request": {
        "act": "rec_load_all",
        "a": "rec_load_all",
        "email": "sample@example.com",
        "tkn": "8afbe6dea02407989af4dd4c97bb6e25",
        "z": "example.com"
    },
    -
    "response": {
        -
        "recs": {
            "has_more": false,
            "count": 7,
            -
            "objs": [
                -
                {
                    "rec_id": "16606009",
                    "rec_tag": "7f8e77bac02ba65d34e20c4b994a202c",
                    "zone_name": "example.com",
                    "name": "direct.example.com",
                    "display_name": "direct",
                    "type": "A",
                    "prio": null,
                    "content": "[server IP]",
                    "display_content": "[server IP]",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "0",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 0,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                },
                -
                {
                    "rec_id": "16606003",
                    "rec_tag": "d5315634e9f5660d3670e62fa176e5de",
                    "zone_name": "example.com",
                    "name": "home.example.com",
                    "display_name": "home",
                    "type": "A",
                    "prio": null,
                    "content": "[server IP]",
                    "display_content": "[server IP]",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "0",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 0,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                },
                -
                {
                    "rec_id": "16606000",
                    "rec_tag": "23b26c051884e94e18711742942760b1",
                    "zone_name": "example.com",
                    "name": "example.com",
                    "display_name": "example.com",
                    "type": "A",
                    "prio": null,
                    "content": "[server IP]",
                    "display_content": "[server IP]",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "1",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 1,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                },
                -
                {
                    "rec_id": "18136402",
                    "rec_tag": "3bcef45cdf5b7638b13cfb89f1b6e716",
                    "zone_name": "example.com",
                    "name": "test.example.com",
                    "display_name": "test",
                    "type": "A",
                    "prio": null,
                    "content": "[server IP]",
                    "display_content": "[server IP]",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "0",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 0,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                },
                -
                {
                    "rec_id": "16606018",
                    "rec_tag": "c0b453b2d94213a7930d342114cbda86",
                    "zone_name": "example.com",
                    "name": "www.example.com",
                    "display_name": "www",
                    "type": "CNAME",
                    "prio": null,
                    "content": "example.com",
                    "display_content": "example.com",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "0",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 0,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                },
                -
                {
                    "rec_id": "17119732",
                    "rec_tag": "1faa40f85c78bccb69ee8116e84f3b40",
                    "zone_name": "example.com",
                    "name": "xn--vii.example.com",
                    "display_name": "⟵",
                    "type": "CNAME",
                    "prio": null,
                    "content": "example.com",
                    "display_content": "example.com",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "1",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 1,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                },
                -
                {
                    "rec_id": "16606030",
                    "rec_tag": "2012b3a2e49978ef18ee13dd98e6b6f7",
                    "zone_name": "example.com",
                    "name": "yay.example.com",
                    "display_name": "yay",
                    "type": "CNAME",
                    "prio": null,
                    "content": "domains.tumblr.com",
                    "display_content": "domains.tumblr.com",
                    "ttl": "1",
                    "ttl_ceil": 86400,
                    "ssl_id": null,
                    "ssl_status": null,
                    "ssl_expires_on": null,
                    "auto_ttl": 1,
                    "service_mode": "0",
                    -
                    "props": {
                        "proxiable": 1,
                        "cloud_on": 0,
                        "cf_open": 1,
                        "ssl": 0,
                        "expired_ssl": 0,
                        "expiring_ssl": 0,
                        "pending_ssl": 0
                    }
                }
            ]
        }
    },
    "result": "success",
    "msg": null

}

3.4 - "zone_check" - Checks for active zones and returns their corresponding zids

Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "zones"

    List of zones separated by comma

Here is an example POST to the zone_check checking for the zids for a list of different zones:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=zone_check' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'zones=example.com,example2.com,example3.com'


Output: 	


{

    -
    "response": {
        -
        "zones": {
            "example1.com": 1,
            "example2.com": 2,
            "example3.com": 3
        }
    },
    "result": "success",
    "msg": null

}
3.5 - "zone_ips" - Pull recent IPs visiting site

Returns a list of IP address which hit your site classified by type.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "hours"

    Past number of hours to query. Default is 24, maximum is 48.
    "class"

    Optional. Restrict the result set to a given class as given by:

        "r" -- regular
        "s" -- crawler
        "t" -- threat
    "geo"

    Optional. Set to 1 to add longitude and latitude information to response

Here is an example POST to the zone_ips action for pulling recent IPs of regular visitors and their location that visited example.com in the last 48 hours:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=zone_ips' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'hours=48' \
  -d 'class=r' \
  -d 'geo=1'


Output: 	


{

    -
    "response": {
        -
        "ips": [
            -
            {
                "ip": "[IP of Visitor]",
                "classification": "regular",
                "hits": 44,
                "latitude": 37.76969909668,
                "longitude": -122.39330291748,
                "zone_name": "example.com"
            },
            -
            {
                "ip": "[IP of Visitor]",
                "classification": "regular",
                "hits": 21,
                "latitude": 37.396099090576,
                "longitude": -121.96170043945,
                "zone_name": "example.com"
            },
            -
            {
                "ip": "[IP of Visitor]",
                "classification": "regular",
                "hits": 8,
                "latitude": 37.76969909668,
                "longitude": -122.39330291748,
                "zone_name": "example.com"
            },
            -
            {
                "ip": "[IP of Visitor]",
                "classification": "regular",
                "hits": 4,
                "latitude": 38,
                "longitude": -97,
                "zone_name": "example.com"
            }
        ]
    },
    "result": "success",
    "msg": null

}

3.6 - "ip_lkup" - Check threat score for a given IP

Find the current threat score for a given IP. Note that scores are on a logarithmic scale, where a higher score indicates a higher threat.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "ip"

    The target IP

Here is an example POST to the ip_lkup for looking up the threat score for an example IP:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=ip_lkup' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'ip=0.0.0.0'


Output: 	


{

    -
    "response": {
        "[IP]": "BAD:0"
    },
    "result": "success",
    "msg": null

}

3.7 - "zone_settings" - List all current setting values

Retrieves all current settings for a given domain.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain

Here is an example POST to the zone_settings for looking up settings for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=zone_settings' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com'


Output: 	


{

    -
    "request": {
        "act": "zone_settings",
        "tkn": "8afbe6dea02407989af4dd4c97bb6e25",
        "a": "zone_settings",
        "z": "example.com",
        "email": "sample@example.com"
    },
    -
    "response": {
        -
        "result": {
            -
            "objs": [
                -
                {
                    "userSecuritySetting": "Medium",
                    "dev_mode": 1345768790,
                    "ipv46": 3,
                    "ob": 1,
                    "cache_lvl": "agg",
                    "outboundLinks": "disabled",
                    "async": "0",
                    "bic": "1",
                    "chl_ttl": "900",
                    "exp_ttl": "7200",
                    "fpurge_ts": "1345757991",
                    "hotlink": "1",
                    "img": "171",
                    "lazy": "1",
                    "minify": "0",
                    "outlink": "0",
                    "preload": "0",
                    "s404": "1",
                    "sec_lvl": "med",
                    "spdy": "1",
                    "ssl": "1",
                    "waf_profile": "high"
                }
            ]
        }
    },
    "result": "success",
    "msg": null

}

Explanation of some of the significant fields

    "dev_mode" -- Development Mode status. A zero response indicates off. A non-zero response indicates on. The numerical value of a non-zero response indicates when Development Mode will expire.

    "ob" -- Always Online status; Values are [0 = off; 1 = on].

    "ch_ttl" -- Challenge TTL, in seconds.

    "exp_ttl" -- Expire TTL (for CloudFlare-cached items), in seconds.

    "sec_lvl" -- Basic Security Level. Values are [eoff (Essentially Off), low, med, high, help (I'm Under Attack!)]

    "cache_lvl" -- Caching Level. Values are: [agg = aggressive, iqs = simplified, basic]

    "async" -- Rocket Loader. Values are: [0 = off, a = automatic, m = manual.]

    "minify" -- Minification
        0 = off
        1 = JavaScript only
        2 = CSS only
        3 = JavaScript and CSS
        4 = HTML only
        5 = JavaScript and HTML
        6 = CSS and HTML
        7 = CSS, JavaScript, and HTML 

    "ipv46" -- IPV6, Values are [0 = off, 3 = Full]

    "bic" -- Browser Integrity Check. Values are [0 = off, 1 = on]

    "email_filter" -- Email obfuscation. Values are [0 = off, 1 = on]

    "sse" -- Server Side Exclude. Values are [0 = off, 1 = on]

    "hotlink" -- Hotlink Protection. Values are [0 = off, 1 = on]

    "geoloc" -- IP Geolocation. Values are [0 = off, 1 = on]

    "spdy" -- [PRO/BUSINESS/ENTERPRISE] SPDY. Values are [0 = off, 1 = on]

    "ssl" -- [PRO/BUSINESS/ENTERPRISE] SSL Status. Values are [0 = off, 1 = Flexible, 2 = Full, 3 = Full (Strict)]

    "lazy" -- [PRO/BUSINESS/ENTERPRISE] Mirage: Lazy Load. Values are [0 = off, 1 = on]

    "img" -- [PRO/BUSINESS/ENTERPRISE] Mirage: Auto-resize, Polish settings
        0 = Off
        1 = Auto-resize on
        200 = Polish: Basic
        201 = Polish: Basic, Mirage: Auto-resize on
        170 = Polish: Basic + JPEG
        171 = Polish: Basic + JPEG, Mirage: Auto-resize on 

    "preload" -- [PRO/BUSINESS/ENTERPRISE] Preloader. Values are [0 = off, 1 = on]

    "waf_profile" -- [PRO/BUSINESS/ENTERPRISE] WAF setting. Values are [high, low, off] 

4 - Modify
4.1 - "sec_lvl" - Set the security level

This function sets the Basic Security Level to I'M UNDER ATTACK! / HIGH / MEDIUM / LOW / ESSENTIALLY OFF.

Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The domain that records are being retrieved from
    "v"

    The security level:
        "help" -- I'm under attack!
        "high" -- High
        "med" -- Medium
        "low" -- Low
        "eoff" -- Essentially Off

Here is an example POST to the sec_lvl action for setting the security level to "I'm under attack!" for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=sec_lvl' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=help'


Output: 	


{

    -
    "response": {
        -
        "zone": {
            -
            "obj": {
                "zone_id": "44",
                "user_id": "1",
                "zone_name": "example.com",
                "display_name": "example.com",
                "zone_status": "V",
                "zone_mode": "1",
                "host_id": "115",
                "zone_type": "P",
                "host_pubname": "Hosting Provider",
                "host_website": "providersite.com",
                "vtxt": null,
                "fqdns": null,
                "step": "4",
                "zone_status_class": "status-active",
                "zone_status_desc": "CloudFlare powered, this website will be accelerated and protected (<a class=\"modal-link-faq muted\" href=\"#\" onClick=\"cloudFlare.faq('en_US', ['CompleteActive']);return false;\">info</a>)",
                "ns_vanity_map": [ ],
                "orig_registrar": null,
                "orig_dnshost": null,
                "orig_ns_names": null,
                -
                "props": {
                    "dns_cname": 0,
                    "dns_partner": 1,
                    "dns_anon_partner": 0,
                    "pro": 0,
                    "expired_pro": 0,
                    "pro_sub": 0,
                    "ssl": 0,
                    "expired_ssl": 0,
                    "expired_rs_pro": 0,
                    "reseller_pro": 0,
                    "force_interal": 0,
                    "ssl_needed": 0,
                    "alexa_rank": 0
                },
                -
                "confirm_code": {
                    "zone_deactivate": "c85831462b7bf79d69ad82aba1fa02ce",
                    "zone_dev_mode1": "fc6cacf3b4ef5bbdb9c0099a9762ae94"
                },
                -
                "allow": [
                    "analytics",
                    "threat_control",
                    "cf_apps",
                    "dns_editor",
                    "cf_settings",
                    "page_rules",
                    "zone_deactivate",
                    "zone_dev_mode1"
                ]
            }
        }
    },
    "result": "success",
    "msg": null

}
4.2 - "cache_lvl" - Set the cache level

This function sets the Caching Level to Aggressive or Basic.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

    The cache level:
        "agg" -- Aggressive
        "basic" -- Basic

Here is an example POST to the cache_lvl action for setting the cache level to "Aggressive" for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=cache_lvl' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=agg'


Output: 	


{

    -
    "response": {
        -
        "zone": {
            -
            "obj": {
                "zone_id": "42",
                "user_id": "1",
                "zone_name": "example.com",
                "display_name": "example.com",
                "zone_status": "V",
                "zone_mode": "1",
                "host_id": "415",
                "zone_type": "P",
                "host_pubname": "SomeHost",
                "host_website": "http://www.SomeHost.com",
                "vtxt": null,
                "fqdns": null,
                "step": "4",
                "zone_status_class": "status-active",
                "zone_status_desc": "CloudFlare powered, this website will be accelerated and protected (<a class=\"modal-link-faq muted\" href=\"#\" onClick=\"cloudFlare.faq('en_US', ['CompleteActive']);return false;\">info</a>)",
                "ns_vanity_map": [ ],
                "orig_registrar": null,
                "orig_dnshost": null,
                "orig_ns_names": null,
                -
                "props": {
                    "dns_cname": 0,
                    "dns_partner": 1,
                    "dns_anon_partner": 0,
                    "pro": 0,
                    "expired_pro": 0,
                    "pro_sub": 0,
                    "ssl": 0,
                    "expired_ssl": 0,
                    "expired_rs_pro": 0,
                    "reseller_pro": 0,
                    "force_interal": 0,
                    "ssl_needed": 0,
                    "alexa_rank": 0
                },
                -
                "confirm_code": {
                    "zone_deactivate": "f91eb75acb516aa95dabf5fa2b7305ec",
                    "zone_dev_mode1": "84fae8e497487d5dea45a39f00054488"
                },
                -
                "allow": [
                    "analytics",
                    "threat_control",
                    "cf_apps",
                    "dns_editor",
                    "cf_settings",
                    "page_rules",
                    "zone_deactivate",
                    "zone_dev_mode1"
                ]
            }
        }
    },
    "result": "success",
    "msg": null

}

4.3 - "devmode" - Toggling Development Mode

This function allows you to toggle Development Mode on or off for a particular domain. When Development Mode is on the cache is bypassed. Development mode remains on for 3 hours or until when it is toggled back off.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

    1 for on, 0 for off

Here is an example POST to the devmode action for turning on Development Mode for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=devmode' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=1'


Output: 	

Development mode will expire on "expires_on" (3 hours from when it is toggled on). Development mode can be toggled off immediately by setting "v" to 0.

{

    -
    "response": {
        "expires_on": 1336697110,
        -
        "zone": {
            -
            "obj": {
                "zone_id": "42",
                "user_id": "1",
                "zone_name": "example.com",
                "display_name": "example.com",
                "zone_status": "V",
                "zone_mode": "1",
                "host_id": "415",
                "zone_type": "P",
                "host_pubname": "SomeHost",
                "host_website": "http://www.SomeHost.com",
                "vtxt": null,
                "fqdns": null,
                "step": "4",
                "zone_status_class": "status-dev-mode",
                "zone_status_desc": "<strong>Development mode</strong> active: CloudFlare's accelerated cache is inactive on this site, so changes to cachable content (like images, CSS, or JavaScript) are visible immediately. Press shift-reload if your changes are not immediate. Development mode will automatically toggle off <strong>3 hours</strong> after initial setup.",
                "ns_vanity_map": [ ],
                "orig_registrar": null,
                "orig_dnshost": null,
                "orig_ns_names": null,
                -
                "props": {
                    "dns_cname": 0,
                    "dns_partner": 1,
                    "dns_anon_partner": 0,
                    "pro": 0,
                    "expired_pro": 0,
                    "pro_sub": 0,
                    "ssl": 0,
                    "expired_ssl": 0,
                    "expired_rs_pro": 0,
                    "reseller_pro": 0,
                    "force_interal": 0,
                    "ssl_needed": 0,
                    "alexa_rank": 0
                },
                -
                "confirm_code": {
                    "zone_deactivate": "f91eb75acb516aa95dabf5fa2b7305ec"
                },
                -
                "allow": [
                    "analytics",
                    "threat_control",
                    "cf_apps",
                    "dns_editor",
                    "cf_settings",
                    "page_rules",
                    "zone_deactivate",
                    "zone_dev_mode0"
                ]
            }
        }
    },
    "result": "success",
    "msg": null

}

4.4 - "fpurge_ts" -- Clear CloudFlare's cache

This function will purge CloudFlare of any cached files. It may take up to 48 hours for the cache to rebuild and optimum performance to be achieved so this function should be used sparingly.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

    Value can only be "1."
    c

Here is an example POST to the fpurge_ts action for purging the cache for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=fpurge_ts' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=1'


Output: 	


{

    -
    "response": {
        "fpurge_ts": 1336705243,
        -
        "zone": {
            -
            "obj": {
                "zone_id": "438",
                "user_id": "1",
                "zone_name": "sample-domain.com",
                "display_name": "sample-domain.com",
                "zone_status": "V",
                "zone_mode": "1",
                "host_id": null,
                "zone_type": "F",
                "host_pubname": null,
                "host_website": null,
                "vtxt": null,
                -
                "fqdns": [
                    "norm.ns.cloudflare.com",
                    "pat.ns.cloudflare.com"
                ],
                "step": "4",
                "zone_status_class": "status-active",
                "zone_status_desc": "CloudFlare powered, this website will be accelerated and protected (<a class=\"modal-link-faq muted\" href=\"#\" onClick=\"cloudFlare.faq('en_US', ['CompleteActive']);return false;\">info</a>)",
                "ns_vanity_map": [ ],
                "orig_registrar": "ENOM, INC.",
                "orig_dnshost": null,
                "orig_ns_names": "{NS1.NAMESERVER.COM,NS2.NAMESERVER.COM}",
                -
                "props": {
                    "dns_cname": 0,
                    "dns_partner": 0,
                    "dns_anon_partner": 0,
                    "pro": 0,
                    "expired_pro": 0,
                    "pro_sub": 0,
                    "ssl": 0,
                    "expired_ssl": 0,
                    "expired_rs_pro": 0,
                    "reseller_pro": 0,
                    "force_interal": 0,
                    "ssl_needed": 0,
                    "alexa_rank": 0
                },
                -
                "confirm_code": {
                    "zone_deactivate": "f7d2ce4b08c1fe4639021b3a16859072",
                    "zone_dev_mode1": "a69cba93651248e15cfa8c0b50c2c418"
                },
                -
                "allow": [
                    "analytics",
                    "threat_control",
                    "cf_apps",
                    "dns_editor",
                    "cf_settings",
                    "page_rules",
                    "zone_deactivate",
                    "zone_dev_mode1"
                ]
            }
        }
    },
    "result": "success",
    "msg": null,
    -
    "attributes": {
        "cooldown": 20
    }

}

4.5 - "zone_file_purge" -- Purge a single file in CloudFlare's cache

This function will purge a single file from CloudFlare's cache.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "url"

    The full URL of the file that needs to be purged from CloudFlare's cache. Keep in mind, that if an HTTP and an HTTPS version of the file exists, then both versions will need to be purged independently

Here is an example POST to the zone_file_purge action for purging the cache for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=zone_file_purge' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'url=http://www.example.com/style.css'


Output: 	


{

    -
    "request": {
        "act": "zone_file_purge",
        "tkn": "8afbe6dea02407989af4dd4c97bb6e25",
        "email": "sample@example.com",
        "a": "zone_file_purge",
        "z": "example.com",
        "url": "http://www.example.com/style.css"
    },
    -
    "response": {
        "vtxt_match": null,
        "url": "http://www.example.com/style.css"
    },
    "result": "success",
    "msg": null

}

4.6 - "zone_grab" - Update the snapshot of site for CloudFlare's challenge page

Tells CloudFlare to take a new image of your site.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "zid"

    ID of zone, found in zone_check

Here is an example POST to the zone_grab action for grabbing a new snapshot for for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=zone_grab' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'zid=42' \


Note: This API call can only be used once per day.
Output: 	


{

    "result": "success",
    "msg": null

}

4.7 - "wl" / "ban" / "nul" -- Whitelist/Blacklist/Unlist IPs

Whitelist with "wl" and Blacklist with "ban", and use "nul" to remove the IP from either of those lists
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "key"

    The IP address you want to whitelist/blacklist

Here is an example POST to the wl action to whitelist an IP:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=wl' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'key=0.0.0.0' \


Output: 	


{

    -
    "response": {
        -
        "result": {
            "ip": "[IP]",
            "action": "WL"
        }
    },
    "result": "success",
    "msg": null

}

{

    -
    "response": {
        -
        "result": {
            "ip": "[IP]",
            "action": "BAN"
        }
    },
    "result": "success",
    "msg": null

}

4.8 - "ipv46" -- Toggle IPv6 support

Toggles IPv6 support
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

    Disable with 0, enable with 3

Here is an example POST to the ipv46 action to turn on IPv6 support for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=ipv46' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=3'


Output: 	


{

    -
    "response": {
        -
        "zone": {
            -
            "obj": {
                "zone_id": "763858",
                "user_id": "39781",
                "zone_name": "example.com",
                "display_name": "example.com",
                "zone_status": "V",
                "zone_mode": "1",
                "host_id": null,
                "zone_type": "F",
                "host_pubname": null,
                "host_website": null,
                "vtxt": null,
                -
                "fqdns": [
                    "norm.ns.cloudflare.com",
                    "pat.ns.cloudflare.com"
                ],
                "step": "4",
                "zone_status_class": "status-active",
                "zone_status_desc": "CloudFlare powered, this website will be accelerated and protected (<a class=\"modal-link-faq muted\" href=\"#\" onClick=\"cloudFlare.faq('en_US', ['CompleteActive']);return false;\">info</a>)",
                "ns_vanity_map": [ ],
                "orig_registrar": "ENOM, INC.",
                "orig_dnshost": null,
                "orig_ns_names": "{NS1.NAMESERVER.COM,NS2.NAMESERVER.COM}",
                -
                "props": {
                    "dns_cname": 0,
                    "dns_partner": 0,
                    "dns_anon_partner": 0,
                    "pro": 0,
                    "expired_pro": 0,
                    "pro_sub": 0,
                    "ssl": 0,
                    "expired_ssl": 0,
                    "expired_rs_pro": 0,
                    "reseller_pro": 0,
                    "force_interal": 0,
                    "ssl_needed": 0,
                    "alexa_rank": 0
                },
                -
                "confirm_code": {
                    "zone_deactivate": "f7d2ce4b08c1fe4639021b3a16859072",
                    "zone_dev_mode1": "a69cba93651248e15cfa8c0b50c2c418"
                },
                -
                "allow": [
                    "analytics",
                    "threat_control",
                    "cf_apps",
                    "dns_editor",
                    "cf_settings",
                    "page_rules",
                    "zone_deactivate",
                    "zone_dev_mode1"
                ]
            }
        }
    },
    "result": "success",
    "msg": null

}

4.9 - "async" -- Set Rocket Loader

Changes Rocket Loader setting
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

    [0 = off, a = automatic, m = manual]

Here is an example POST to the async action to set Rocket Loader to "automatic" for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=async' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=a'


Output: 	


{

    "result": "success",
    "msg": null

}

4.10 - "minify" -- Set Minification

Changes minification settings
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

        0 = off
        1 = JavaScript only
        2 = CSS only
        3 = JavaScript and CSS
        4 = HTML only
        5 = JavaScript and HTML
        6 = CSS and HTML
        7 = CSS, JavaScript, and HTML 

Here is an example POST to the minify action to set minifcation for CSS and HTML for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=minify' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=6'


Output: 	


{

    "result": "success",
    "msg": null

}

4.11 - "mirage2" -- Set Mirage2

Toggles mirage2 support
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "v"

        0 = off
        1 = on 

Here is an example POST to the mirage2 action to turn on mirage2 support for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=mirage2' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'v=1'


Output: 	


{

    "result": "success",
    "msg": null

}

5 - DNS Record Management
5.1 - "rec_new" -- Add a DNS record

Create a DNS record for a zone
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "type"

    Type of DNS record. Values include: [A/CNAME/MX/TXT/SPF/AAAA/NS/SRV/LOC]
    "name"

    Name of the DNS record.
    "content"

    The content of the DNS record, will depend on the the type of record being added
    "ttl"

    TTL of record in seconds. 1 = Automatic, otherwise, value must in between 120 and 86400 seconds.
    "prio"[applies to MX/SRV]

    MX record priority.
    "service"[applies to SRV]

    Service for SRV record
    "srvname"[applies to SRV]

    Service Name for SRV record
    "protocol"[applies to SRV]

    Protocol for SRV record. Values include: [_tcp/_udp/_tls].
    "weight"[applies to SRV]

    Weight for SRV record.
    "port"[applies to SRV]

    Port for SRV record
    "target"[applies to SRV]

    Target for SRV record

Here is an example POST to the rec_new action to create an A record for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=rec_new' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'type=A'
  -d 'name=sub'
  -d 'content=1.2.3.4'


Output: 	


{

    -
    "request": {
        "act": "rec_new",
        "a": "rec_new",
        "tkn": "8afbe6dea02407989af4dd4c97bb6e25",
        "email": "sample@example.com",
        "type": "A",
        "z": "example.com",
        "name": "test",
        "content": "96.126.126.36",
        "ttl": "1",
        "service_mode": "1"
    },
    -
    "response": {
        -
        "rec": {
            -
            "obj": {
                "rec_id": "23734516",
                "rec_tag": "b3db8b8ad50389eb4abae7522b22852f",
                "zone_name": "example.com",
                "name": "test.example.com",
                "display_name": "test",
                "type": "A",
                "prio": null,
                "content": "96.126.126.36",
                "display_content": "96.126.126.36",
                "ttl": "1",
                "ttl_ceil": 86400,
                "ssl_id": "12805",
                "ssl_status": "V",
                "ssl_expires_on": null,
                "auto_ttl": 1,
                "service_mode": "0",
                -
                "props": {
                    "proxiable": 1,
                    "cloud_on": 0,
                    "cf_open": 1,
                    "ssl": 1,
                    "expired_ssl": 0,
                    "expiring_ssl": 0,
                    "pending_ssl": 0,
                    "vanity_lock": 0
                }
            }
        }
    },
    "result": "success",
    "msg": null

}

5.2 - "rec_edit" -- Edit a DNS record

Edit a DNS record for a zone. The record will be updated to the data passed through arguments here.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "type"

    Type of DNS record. Values include: [A/CNAME/MX/TXT/SPF/AAAA/NS/SRV/LOC]
    "id"

    DNS Record ID. Available by using the rec_load_all call.
    "name"

    Name of the DNS record.
    "content"

    The content of the DNS record, will depend on the the type of record being added
    "ttl"

    TTL of record in seconds. 1 = Automatic, otherwise, value must in between 120 and 86400 seconds.
    "service_mode"[applies to A/AAAA/CNAME]

    Status of CloudFlare Proxy, 1 = orange cloud, 0 = grey cloud.
    "prio"[applies to MX/SRV]

    MX record priority.
    "service"[applies to SRV]

    Service for SRV record
    "srvname"[applies to SRV]

    Service Name for SRV record
    "protocol"[applies to SRV]

    Protocol for SRV record. Values include: [_tcp/_udp/_tls].
    "weight"[applies to SRV]

    Weight for SRV record.
    "port"[applies to SRV]

    Port for SRV record
    "target"[applies SRV]

    Target for SRV record

Here is an example POST to the rec_edit action to edit an A record for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=rec_edit' \
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'id=9001' \
  -d 'email=sample@example.com' \
  -d 'z=example.com' \
  -d 'type=A'
  -d 'name=sub'
  -d 'content=1.2.3.4'
  -d 'service_mode=1'
  -d 'ttl=1'


Output: 	


{

    -
    "request": {
        "act": "rec_edit",
        "a": "rec_edit",
        "tkn": "1296c62233d48a6cf0585b0c1dddc3512e4b2",
        "id": "23734516",
        "email": "sample@example.com",
        "type": "A",
        "z": "example.com",
        "name": "sub",
        "content": "96.126.126.36",
        "ttl": "1",
        "service_mode": "1"
    },
    -
    "response": {
        -
        "rec": {
            -
            "obj": {
                "rec_id": "23734516",
                "rec_tag": "b3db8b8ad50389eb4abae7522b22852f",
                "zone_name": "example.com",
                "name": "sub.example.com",
                "display_name": "sub",
                "type": "A",
                "prio": null,
                "content": "96.126.126.36",
                "display_content": "96.126.126.36",
                "ttl": "1",
                "ttl_ceil": 86400,
                "ssl_id": "12805",
                "ssl_status": "V",
                "ssl_expires_on": null,
                "auto_ttl": 1,
                "service_mode": "1",
                -
                "props": {
                    "proxiable": 1,
                    "cloud_on": 1,
                    "cf_open": 0,
                    "ssl": 1,
                    "expired_ssl": 0,
                    "expiring_ssl": 0,
                    "pending_ssl": 0,
                    "vanity_lock": 0
                }
            }
        }
    },
    "result": "success",
    "msg": null

}

5.3 - "rec_delete" -- Delete a DNS record

Delete a record for a domain.
Input: 	

Requires the basic parameters described in Section 2.1 of this document. In addition you must pass the following parameters:

    "z"

    The target domain
    "id"

    DNS Record ID. Available by using the rec_load_all call.

Here is an example POST to the rec_delete action to edit A DNS record for example.com:

curl https://www.cloudflare.com/api_json.html \
  -d 'a=rec_delete'
  -d 'tkn=8afbe6dea02407989af4dd4c97bb6e25' \
  -d 'email=sample@example.com' \
  -d 'z=example.com'
  -d 'id=9001'


Output: 	


{

    -
    "request": {
        "act": "rec_delete",
        "a": "rec_delete",
        "tkn": "1296c62233d48a6cf0585b0c1dddc3512e4b2",
        "id": "23735515",
        "email": "sample@example.com",
        "z": "example.com"
    },
    "result": "success",
    "msg": null

}

Copyright © 2012 CloudFlare, Inc. All rights reserved.

***************************
