---
layout: post
title:  "Aztek 2-Day Innovation Day!"
date:   2021-03-26 11:15:00 -0400
categories: innoviation
---
# Introduction - Demo
For the first time, Aztek held a two-day Innovation Day. Here's a high-level summary of what I accomplished:
- <i class="icon-book"></i><i class="icon-check"></i> Learn [Azure Prepaid Compute](#azure-prepaid-compute) at 40% savings (Day 1)
- <i class="icon-hand-right"></i><i class="icon-check"></i> [Static site generators](#static-site-generators) - Jekyll / GitHub Pages / Ruby (Day 1)
- <i class="icon-hand-right"></i><i class="icon-check"></i> [HSTS](#hsts) Aztek Template Project and AztekWeb.com - HSTS
- <i class="icon-check"></i> [Umbraco Unattended Installs](#umbraco-unattended-installs) - AztekWeb.com Upgrade

## Azure Prepaid Compute
* Moved the dev and uat servers to a new prepaid plan to save approx 40%
* Took a good amount of reading about the different levels, restrictions and options
* Read about Azure File Storage; currently our servers are not on the levels that qualifiy for discounts

References
* [](https://docs.microsoft.com/en-us/azure/virtual-machines/prepay-reserved-vm-instances#buy-a-reserved-vm-instance)

## Static Site Generators
Wow. How awesome is this blog? Markdown. GitHub. No CSS? Brilliant!
* Learned about Jekyll, templating, static HTML generation, font awesome, and GitHub Pages
* Uninstalled and reinstalled about 5 times
* More command line; more compile issues; another local webserver?
* Ruby and Gems! 

## HSTS
* Updated our default template for Umbraco automation
* Moved the HSTS header (found the localhost SSL issue)
* Updated AztekWeb.com
* Rule order change and the outbound header addition with a negate for localhost

## Umbraco Unattended Installs



