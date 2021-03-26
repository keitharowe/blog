---
layout: post
title:  "Aztek 2-Day Innovation Day!"
date:   2021-03-26 11:15:00 -0400
categories: innoviation
---
# Introduction - Demo
For the first time, Aztek held a two-day Innovation Day. Here's a high-level summary of what I accomplished:
- <i class="icon-book"></i>Learn<i class="icon-check"></i> [Azure Prepaid Compute](#azure-prepaid-compute) at 40% savings (Day 1)
- <i class="icon-hand-right"></i>Build<i class="icon-check"></i> [Static site generators](#static-site-generators) - Jekyll / GitHub Pages / Ruby (Day 1)
- <i class="icon-hand-right"></i>Build<i class="icon-check"></i> [HSTS](#hsts) Aztek Template Project and AztekWeb.com - HSTS
- <i class="icon-smile">Do<i class="icon-check"></i> [Umbraco Unattended Installs](#umbraco-unattended-installs) - AztekWeb.com Upgrade

## Azure Prepaid Compute
Microsoft would rather have customers on a prepaid plan for numerous reasons. When moving to the new plans they offer significant discounts on the main "cloud" functions such as virtal machines or compute. We moved our core dev and uat infrastructure over to this new prepaid plan.
* Moved the dev and uat servers to a new prepaid plan to save approx 40%
* Took a good amount of reading about the different levels, restrictions and options
* Read about Azure File Storage; currently our servers are not on the levels that qualifiy for discounts

### References
* [https://docs.microsoft.com/en-us/azure/virtual-machines/prepay-reserved-vm-instances#buy-a-reserved-vm-instance](https://docs.microsoft.com/en-us/azure/virtual-machines/prepay-reserved-vm-instances#buy-a-reserved-vm-instance)

## Static Site Generators
Wow. How awesome is this blog? Markdown. GitHub. No CSS? Brilliant!
* Learned about Jekyll, templating, static HTML generation, font awesome, and GitHub Pages
* Uninstalled and reinstalled about 5 times
* More command line; more compile issues; another local webserver?
* Ruby and Gems! 
* GitHub built in worfklows for publishing and free hosting
* Maybe try a custom domain next?
* <span class="icon-heart"></span> Markdown

### References
* [https://jekyllrb.com/docs/](https://jekyllrb.com/docs/)
* [Markdown Guide](https://www.markdownguide.org/basic-syntax/)
* [GitHub Pages](https://pages.github.com/)

## HSTS
What is HSTS? HSTS stands for HTTP Strict Transport Security. Why is this important? HSTS helps to protect websites against man-in-the-middle attacks such as protocol downgrade attacks and cookie hijacking. It allows web servers to declare that web browsers (or other complying user agents) should automatically interact with it using only HTTPS connections.<sup>1</sup>

* Updated our default template for Umbraco automation by creating a pull request
* Moved the HSTS header (found the localhost SSL issue)
* Updated AztekWeb.com / deployed to production
* Rule order change and the outbound header addition with a negate for localhost
* HSTS not supported in our "core" Windows Hosting Enviornment; need Windows Server 2019 and higher. However, this is baked in finally! 

Basically, one needs to force https THEN to the canonical to satisify the requirement for HSTS. As such I shuffled the rules. So http://aztekweb.com needs to to to https://aztekweb.com then to the canonical. 

Here is what the new rules look like:

      <rules>
        <rule name="AddTrailingSlashRule">
          <match url="(.*[^/])$"/>
          <conditions>
            <add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true"/>
            <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true"/>
            <add input="{URL}" pattern="^.*\.(axd|asmx|xml|aspx|pdf|jpg|png)$" negate="true"/>
            <add input="{REQUEST_URI}" pattern="^/umbraco/" negate="true"/>
            <add input="{REQUEST_URI}" pattern="^/install/" negate="true"/>
            <add input="{REQUEST_URI}" pattern="^/.well-known/" negate="true"/>
          </conditions>
          <action type="Redirect" url="{R:1}/" redirectType="Permanent"/>
        </rule>
        <rule name="Rewrite Map Rule" stopProcessing="false">
          <conditions>
            <add input="{Static301Redirects:{PATH_INFO}}" pattern="(.+)"/>
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}{C:1}" appendQueryString="true" redirectType="Permanent"/>
        </rule>
        <rule name="Force HTTPS">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTP_HOST}" pattern="localhost" negate="true"/>
            <add input="{REQUEST_URI}" pattern="^/.well-known/" negate="true"/>
            <add input="{HTTPS}" pattern="off"/>
          </conditions>
          <action type="Redirect" url="https://{HTTP_HOST}/{R:1}" appendQueryString="true" redirectType="Permanent"/>
        </rule>
       <rule name="CanonicalHostNameRule">
          <match url="(.*)"/>
          <conditions>
            <add input="{HTTP_HOST}" pattern="localhost" negate="true"/>
            <add input="{HTTP_HOST}" pattern="aztekhq.com" negate="true"/>
            <add input="{HTTP_HOST}" pattern="^www\.aztekweb\.com$" negate="true"/>
            <add input="{REQUEST_URI}" pattern="^/.well-known/" negate="true"/>
          </conditions>
          <action type="Redirect" url="https://www.aztekweb.com/{R:1}"/>
        </rule>
      </rules>

New rule added for outbound rule:

      <outboundRules>
        <rule name="Add the STS header in HTTPS responses">
          <match serverVariable="RESPONSE_Strict_Transport_Security" pattern=".*"/>
          <conditions>
            <add input="{HTTP_HOST}" pattern="localhost" negate="true"/>
            <add input="{HTTPS}" pattern="on"/>
          </conditions>
          <action type="Rewrite" value="max-age=31536000; includeSubDomains; preload"/>
        </rule>
      </outboundRules>


### Bogus rule found in Umbraco Template project

        <remove name="Strict-Transport-Security"/>
        <add name="Strict-Transport-Security" value="max-age=10886400"/>

### References 
* [HSTS Preload check and validator](https://hstspreload.org/)
* [https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/hsts](https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/hsts)
* [https://docs.microsoft.com/en-us/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts](https://docs.microsoft.com/en-us/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts)

1. [Wikipedia, the free encyclopedia](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)

## Umbraco Unattended Installs
Reviewed how Umbraco has added support for auto-upgrades. Sarah and I did not do the auto-install feature however. 


