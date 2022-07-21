# Isaiah’s Principles of Product Security
## Priority One is to try to stop the bleeding.
- Leverage DevSecOps and CI/CD to provide more security information earlier in the dev process.
- This is a significant first hill to climb and can be highly team-specific and time-consuming.
## A security mindset is far more scalable and efficient than a security staff
- Security consulting with engineering is an opportunity to spread the security mindset and develop buy-in from teams.
- Too many technology organizations, security debt, is grouped with general tech debt. There is no additional allocation of time, money, or resources for getting to these sooner. To engineers, this means it competes with their own desired fixes and prioritization. Engineer buy-in is critical to getting security work prioritized by the team and given the attention it deserves.
- Developers with a security mindset help spread it within their team and are more likely to engage the security team.
## Product Security is not Red-Tape
- We need to run at the pace of the development teams whenever possible.
- Priority one is integrating with the CI/CD so scans are up to date with their pace and they can make informed release decisions.
- Product security is about making security decisions in the context of the product and what is practical for the business and technically sound.
- Product Security is not the same as compliance, although they often take responsibility in this area. We should prioritize practical security over compliance when possible.
- Enforcing compliance requirements can lead to friction with engineering teams and make it more difficult for them to accept the work you are suggesting will genuinely add value.
## The Product should be considered holistically.
- Being too narrow leads to missing vulnerabilities in the meeting points and 
- Narrow security considerations lead to devoting too many resources to sufficiently mitigated vulnerabilities.
- Pair individuals with different areas to cover the entire product together synchronously. This conversation helps lead to additional security benefits as well as cross-training. 
## Great is the enemy of good and good enough.
- Focus on what is practical and reasonable
- Having a consolidated view of vulns is more important than having the best of each scanner. All-in-one tools like Fortify, Veracode, and Qualys may not be the best in class for anyone scanner type. Still, the ability to quickly manage vulns of all scan types with little integration efforts to standardize and import findings is worth the quality difference for still maturing programs.
## Metrics might be dangerous
- Metrics in product security can often be arbitrary and don’t accurately reflect the impact of the product security engineer’s contributions to the company.
- We know that SAST products average about 30% false positives resulting in increased vulnerability counts.
- The rise of Dependency scanning has led to an exponential increase in the number of vulnerabilities in a system.
- Security engineers can’t force engineers to merge their patches or work on remediations and shouldn’t be held accountable for making the vuln count go down but enabling developers to drive it down.
- If you need metrics, you should use them for directionality, not the specific numbers.
- A better idea for some metrics, as suggested by Julian Cohen
  - Vulnerability Lifetime in Production
    - The goal of product security engineering and shift left is to reduce this time to as close to zero as possible.
  - Number of Shadow/Untracked IT projects discovered 
    - We can’t secure what we don’t know about, so identifying gaps in coverage by security is critical. 
    - A critical component to accomplishing this is building trust and relationships with the other relevant teams to engage security proactively. If dealing with security is difficult, they will not want to bring this to the team.


## References
- [Rugged Software Methodology](https://ruggedsoftware.org)
- DevSecOps (Multiple Sources)
- [Julian Cohen’s Product Security Framework](https://hockeyinjune.medium.com/product-security-14127b5838ba)
- [Lcamtuf’s Blog on getting product security engineering right](https://lcamtuf.blogspot.com/2018/02/getting-product-security-engineering.html)
