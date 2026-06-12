# Discussion: integrating affiliate platforms on your website

hosted by @TimoReckling

## challenges when tasked to integrate an affiliate or customer referral a platform on your (client's) website
- personal opinions on tracking and cookies might contrast 
- inexperience with the platform and its documentation and how it relates to
- ambiguous or confusing platform documentation
- platform company rather lax on consent management and privacy compliance
- last interaction or first interact, what to count as the conversion event, and how to track it?

## Google Tag manager and Google Analytics often preferred by marketing teams even if other solutions exist
- pick your battles as a dev whether to try and change it
- potentially unclear ownership of implementation and configuration within the organization
- cross session tracking and cookie management can become complicated and might require a lot of testing and debugging
    - server-side tracking (sst) is the trend and typical solution: first party cookies instead of third party cookies
    - (!) possible legal implications regarding responsibility of third party content showing as content coming from your domain

## Security and bundle size implications of integrating third party scripts and platforms
- third party scripts can introduce security vulnerabilities and performance issues
- bundle size optimizations of own code undermined by size of code loaded by third party scripts
