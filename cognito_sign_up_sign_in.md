## Cognito Sign Up / Sign In

Main aim of this article is to exemplify a simple registration and login with AWS. We'll use Cognito, Lambda and DynamoDB. 

### What is Cognito ? 
It's an authentication and authorization service. What's the difference ? Well, *authentication* authenticates, *authorization* authorizes. 

What I mean is, if you sign in with your username and password, you *authenticate* yourself into the system.This means the system agrees you are a valid identity, but not necessarily one that is *authorized* to access certain parts of it. *Authorization* is your permission to see or access certain parts of the system you're *authenticated* into.

There are two main parts of Cognito, one is **User Pools**, the other is **Federated Identities**. 

**User Pools** handle *authentication*. It supports pretty much everything you can think of, sign up, sign in with or without third party services (Facebook, Google, Amazon etc.), multi factor authentication, message verification, analytics and it can be scaled with lambda triggers (which we'll cover later on). Any successful signin attempt with **User Pools** provides a session token.

**Federated Identities** handle *authorization*. This component of Cognito doesn't support *authentication*, therefore it relies on separate *authentication providers* that can be anything from an already created **User Pool** to *SAML* and even a custom authentication provider. What this means is that another *authentication provider* will issue a session token, like **User Pools** for example, then **Federated Identities** consumes that token and exchanges it with *AWS Credentials* which, in return, provide access to AWS Services. 


