# SSDLC Phase 1: User and Security Requirements
### 3/23/2025
([Key Takeaways at the end of the post](#key-takeaways))

In the last blog post, Starting the Journey, I wrote about an application that could help me design outfits. Today, I want to perform the first step of the SSDLC, which is generating requirements and performing a risk assessment. For reference, I am following [GeeksforGeeks article on the SSDLC](https://www.geeksforgeeks.org/ethical-hacking/what-is-secure-software-development-life-cycle-ssdlc/) which has the following phases: Risk Assessment, Threat Modeling & Design Review, Static Analysis, Security Testing & Code Review, and Security Assessment & Secure Configuration. I wrote about what features I wanted the application to have, which makes defining functional requirements straightforward. 

## Functional Requirements
- [ ] Users should be able to sign up for an account which links their online wardrobe to their account
- [ ] Users should be able to log into their account which then lets them access their saved wardrobe
- [ ] Users should be able to upload pictures of their clothing which is then saved to their online wardrobe. 
- [ ] Users should be able to add attributes of the specific piece of clothing, such as type and color, when uploading their clothing. 
- [ ] Users should be able to build an outfit by selecting a piece of clothing for each type (shirts, pants, shoes, etc.)

Now to create the security requirements, I knew they needed to stem from the functional requirements. For my first pass at the security requirements, I tried thinking like an attacker and considered how I might exploit each piece of functionality listed in the requirements. 

## Security Requirements
- [ ] The system shall validate user input when taking information for sign ups and logins (injection attacks)
- [ ] The system shall validate the file type of uploaded pictures of clothing (RCE depending on how images are processed/utilized)
- [ ] The system shall limit the file size of uploaded pictures of clothing (Denial of Service)

From here, I decided to use AI, Claude, to see how I could polish both my requirements and my process. I prompted it to act as a mentor that could guide me as I navigated my way through the SDLC. I was posed the following:

```
A challenge for you:
You have a requirement around login — but your security requirements only mention input validation for that flow. Think about the login feature more holistically.

What happens after a user successfully authenticates? How does the system know on subsequent requests that the user is who they say they are?
What could an attacker do to someone else's account or wardrobe if that mechanism isn't secured properly?
Your wardrobe is tied to a specific user — what security property needs to hold true every time a user tries to access or modify their data?
```

With these questions, I came up with the additional following security requirements:
- [ ] The system shall ensure users can only view their own wardrobe, and not other users'
- [ ] The system shall ensure users cannot edit other users wardrobes
- [ ] The system shall ensure users cannot add to other users wardrobes
- [ ] The system shall ensure users canot remove from other users wardobes

I asked Claude to reveal the ones they were trying to get with authentication and subsequirent requests:
- [ ] The system shall invalidate session tokens server-side upon logout
- [ ] The system shall expire session tokens after a defined period of inactivity
- [ ] The system shall transmit session tokens only over HTTPS

Throughout this process, I felt like I was doing Threat Modeling instead of a Risk Assessment. I did some research and stumbled upon this enlightening article from Practical DevSecOps: ["Threat Modeling vs Risk Assessment: Understanding the Difference"](https://www.practical-devsecops.com/threat-modeling-vs-risk-assessment/?srsltid=AfmBOoq27ncDNPHF6whkajdx2HFfPiRxYOoQgpHoqPHScHjCg4Iwbi69)

The main takeaway is that Risk Assessments focus on the potential impacts on the organization if assessed threats exploit vulnerabilities and evaluating the likelihood and probability. My misconception was that the generation of security requirements was the process of performing a Risk Assessment, but generating security requirements is it's own thing. One framework that could be used for conducting a risk assessment is going through the following steps: 

1. Identifying Assets and Business Objectives
2. Assessing Vulnerabilities and Threats
3. Determining Potential Impact
4. Evaulating Likelihood and Probability
5. Implementing Risk Mitigation Strategies

So for my application, a risk assessment would look something like this

### Identifying Assets and Business Objectives
The assets for this project would be a server that would be processing outfits and the database that would hold user wardrobes and user login data. The business objective is to keep these assets running in order to provide the users with the service of being able to create and customize outfits. 

### Assessing Vulnerabilities and Threats
The most likely threats that this app would experience if it were to be internet-facing are bots and possibly random script-kiddies. Some examples of vulnerabilities are the ones that the security requirements aim to mitigate such as injection attacks, denial of service on the database, and unauthorized CRUD operations. 

### Determining Potential Impact
Because user data would be stored on the application, regulatory compliance might be an issue such as the GDPR. 

### Evaulating Likelihood and Probability
I don't have any historical data on the effectiveness of bots compromising personal projects, but I would have to assume it is on the lower side. 

### Implementing Risk Mitigation Strategies
These will primarily be technical controls. 

With my misconception cleared up, I have now completed a risk assessment and have generated the following requirements:  


## Functional Requirements
- [ ] Users should be able to sign up for an account which links their online wardrobe to their account
- [ ] Users should be able to log into their account which then lets them access their saved wardrobe
- [ ] Users should be able to upload pictures of their clothing which is then saved to their online wardrobe. 
- [ ] Users should be able to add attributes of the specific piece of clothing, such as type and color, when uploading their clothing. 
- [ ] Users should be able to build an outfit by selecting a piece of clothing for each type (shirts, pants, shoes, etc.)

## Security Requirements
- [ ] The system shall validate user input when taking information for sign ups and logins (injection attacks)
- [ ] The system shall validate the file type of uploaded pictures of clothing (RCE depending on how images are processed/utilized)
- [ ] The system shall limit the file size of uploaded pictures of clothing (Denial of Service)
- [ ] The system shall ensure users can only view their own wardrobe, and not other users'
- [ ] The system shall ensure users cannot edit other users wardrobes
- [ ] The system shall ensure users cannot add to other users wardrobes
- [ ] The system shall ensure users canot remove from other users wardobes
- [ ] The system shall invalidate session tokens server-side upon logout
- [ ] The system shall expire session tokens after a defined period of inactivity
- [ ] The system shall transmit session tokens only over HTTPS

# Key Takeaways
- Risk Assessments are the security step for the Requirements phase and focuses on potential impacts on the organization based on threats and vulnerabilities. 
- Security requirements play no part in the Risk Assessment.
- Security requirements can be done without doing a threat model through the use of established catalogs like the OWASP ASVS and aplying general secure design principles.
- Threat modeling also generates security requirements, but they are specific to the system's architecuture and data flows. 