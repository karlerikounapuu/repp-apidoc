--- 

title: DomainNameRegistry (params in:formData) 

language_tabs: 
   - shell 

toc_footers: 
   - <a href='#'>Sign Up for a Developer Key</a> 
   - <a href='https://github.com/lavkumarv'>Documentation Powered by lav</a> 

includes: 
   - errors 

search: true 

--- 

# Introduction 


<p>Another API description</p>
 

**Version:** 1.0 

# /REPP/V1/DOMAINS
## ***POST*** 

**Description:** 
<p>Creates new domain</p>


### HTTP Request 
`***POST*** /repp/v1/domains` 

**Parameters**

| Name | Located in | Description | Required | Type |
| ---- | ---------- | ----------- | -------- | ---- |
| domain[period_unit] | formData | Period type (month m) or (year y) | Yes | string |
| domain[period] | formData | Registration period in months or years | Yes | number |
| domain[registrant_id] | formData | Registrant contact code | Yes | string |
| domain[name] | formData | Domain name to be registered | Yes | string |
| domain[nameservers_attributes] | formData | Domain nameservers | No | array |
| domain[admin_domain_contacts_attributes] | formData | Tech domain contacts codes | No | array |
| domain[dnskeys_attributes] | formData | DNSSEC keys for domain | No | array |

**Responses**

| Code | Description |
| ---- | ----------- |
| 200 | Verification of domain registration |

<!-- Converted with the swagger-to-slate https://github.com/lavkumarv/swagger-to-slate -->
