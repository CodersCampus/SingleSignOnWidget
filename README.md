# SingleSignOnWidget
Single Sign On Widget attempts to provide a single widget for all apps and apis in the entire family of CodersCampus apps and REST APIs

This page attempts to provide a high level FAQ style description of this widget.

For more detailed information, see [Docs](docs/index.md)

## Frequently Asked Questions (FAQ)

Here are some of the basics you might care to know:

1. [How do we benefit from the Single Sign-On Widget at CodersCampus?](#how-do-we-benefit-from-the-single-sign-on-widget-at-coderscampus)
2. [How many apps do we maintain?](#how-many-apps-do-we-maintain)
3. [Doesn't each app need its own login/registration?](#doesnt-each-app-need-its-own-loginregistration)
4. [What is an app and what is an API in the context of this sign-on?](#what-is-an-app-and-what-is-an-api-in-the-context-of-this-sign-on)
5. [Can I use this to simplify my own Final Project or other designs?](#can-i-use-this-to-simplify-my-own-final-project-or-other-designs)
6. [This seems complicated. Doesn't that make it hard to maintain?](#this-seems-complicated-doesnt-that-make-it-hard-to-maintain)
7. [What if we want to add other federated logins like Github or Facebook?](#what-if-we-want-to-add-other-federated-logins-like-github-or-facebook)
8. [What if apps are Thymeleaf or plain HTML instead of React?](#what-if-apps-are-thymeleaf-or-plain-html-instead-of-react)
9. [What if we want to change authentication providers such as from Firebase to Okta?](#what-if-we-want-to-change-authentication-providers-such-as-from-firebase-to-okta)
10. [Can I change a student's role in one place and have it ripple to all API consumers?](#can-i-change-a-students-role-in-one-place-and-have-it-ripple-to-all-api-consumers)
11. [Can I consume the widget as either regular HTML or a React component?](#can-i-consume-the-widget-as-either-regular-html-or-a-react-component)
12. [Which app actually manages the roles for each user?](#which-app-actually-manages-the-roles-for-each-user)
13. [What if authorization is identity-based in addition to or instead of role-based?](#what-if-authorization-is-identity-based-in-addition-to-or-instead-of-role-based)
14. [Code examples for standard API controllers and standard record-based security?](#code-examples-for-standard-api-controllers-and-standard-record-based-security)
15. [Do JWT custom claims have to anticipate the APIs that consume their roles?](#do-jwt-custom-claims-have-to-anticipate-the-apis-that-consume-their-roles)
16. [What does the app have to do to consume the widget?](#what-does-the-app-have-to-do-to-consume-the-widget)
17. [What does the API have to do to consume the JWT?](#what-does-the-api-have-to-do-to-consume-the-jwt)
18. [Can this be easily extended to apps and APIs not mentioned herein?](#can-this-be-easily-extended-to-apps-and-apis-not-mentioned-herein)
19. [Can these features be added incrementally instead of all in a big effort?](#can-these-features-be-added-incrementally-instead-of-all-in-a-big-effort)
20. [Will this architecture pass stringent modern-day security requirements?](#will-this-architecture-pass-stringent-modern-day-security-requirements)
21. [What if I want to use this in my own SSG like Next, Nuxt, 11ty, or Jekyll?](#what-if-i-want-to-use-this-in-my-own-ssg-like-next-nuxt-11ty-or-jekyll)
22. [I'm confusing Authentication Providers with Sign-Ons Like Google/Facebook/Github](#im-confusing-authentication-providers-with-sign-ons-like-googlefacebookgithub)
23. [How are bootcamp students harmed by tightly coupled security app/API combos?](#how-are-bootcamp-students-harmed-by-tightly-coupled-security-appapi-combos)

Questions not answered:
- What is an html slot?
- How much does an authentication vendor cost?
- How to integrate identity from the signon app in a form?
- Can I use this system without JWT based authorization on the API side?
- Holy cow! Our widget is open sourced with Firebase credentials within it! Couldn't anyone use our firebase account then?
- Is there a Firebase deployment checklist that goes with this?
- Which Firebase offerings are consumed within this widget?

## How do we benefit from the Single Sign-On Widget at CodersCampus?

**Q: How do we benefit from the Single Sign-On Widget at CodersCampus?**

A: The Single Sign-On Widget simplifies the authentication process for users by allowing them to log in once and access multiple apps and services without the need for separate logins. 

A fact that is less obvious just has to do with maintainability. Having to maintain separate security systems for each of our apps tends to make it harder for us to build apps for the convenience of our students - sometimes preventing it entirely.

## How many apps do we maintain?

**Q: How many apps do we maintain?**

A: We maintain approximately 6 apps that could or would be usable by students when we can secure them appropriately.

There are probably a few others that would be considered just because they are easy and dumb, but up to now security has been the biggest obastacle.

So it remains a possibility that an easy to use security mechanism for both the front and back end would release a lot of creativity, internally. Remains to be seen, of course.

## Doesn't each app need its own login/registration?

**Q: Doesn't each app need its own login/registration?**

A: No, the Single Sign-On Widget eliminates the need for separate login and registration processes for each app. 

The front end needs to know that the user is who he says he is, and that somehow he can trust the back end to secure the data.

The back end makes the real security decisions, and it gets all of this information from the JWT. It can validate that the JWT hasn't been faked, so it knows that it can trust the JWT claims.

CORS keeps the conversation only between trusted domains.

And none of the above depends on our back end API server maintaining it's own login/registration. Nice, huh?

## What is an app and what is an API in the context of this sign-on?

**Q: What is an app and what is an API in the context of this sign-on?**

A: In this context, an "app" refers to the user-facing application that users interact with, while an "API" (Application Programming Interface) is the backend service that provides data and functionality to the apps. The Single Sign-On Widget allows users to access both apps, and through the app's JWTs, the relevant APIs, securely.

An example would be a React app running on one server as static code, while fetching it's dynamic data from the API server, which is in turn connected to one or more databases, internally.

## Can I use this to simplify my own Final Project or other designs?

**Q: Can I use this to simplify my own Final Project or other designs?**

A: Yes, you can use the Single Sign-On Widget to simplify the authentication and authorization process in your own projects. It provides a convenient way to implement secure user access.

You would still have to provide your own Firebase account and Firebase project with Authentication. But this is will run on Firebase's free tier, unless of course you expect a massive amount of traffic on your deployed Final Project.

You would also still need to use a service such as Railway or Heroku or [your favorite here] to deploy your java API or HTML server.

## This seems complicated. Doesn't that make it hard to maintain?

**Q: This seems complicated. Doesn't that make it hard to maintain?**

A: While the architecture may appear complex, it is designed to minimize most of the complexity, by farming the complex parts out to an authentication vendor who might have dozens of full time staffers maintaining those complex codebases. 

By farming the most complex pieces out to an authentication vendor, we can provide flexibility and security with a minimal amount of fuss. 

Proper documentation and best practices can help ensure easy maintenance.

## What if we want to add other federated logins like Github or Facebook?

**Q: What if we want to add other federated logins like Github or Facebook?**

A: The Single Sign-On Widget can be extended to support additional federated login providers. 

One can customize the Firebase authentication process to integrate with federated OAuth platforms like 

- Log in with Google (our default)
- Log in with Github (_worth considering, right?_)

Additionally we could, but we probably would not offer these features for various reasons.

- Log in with Facebook
- Log in with Apple
- Log in with Microsoft
- Log in with Twitter
- Log in with phone number
- Log in with email & password

And last, but hardly worth mentioning ...
- Log in with our own, or any other, _custom_ OAuth provider

## What if apps are Thymeleaf or plain HTML instead of React?

**Q: What if apps are Thymeleaf or plain HTML instead of React?**

A: To the consuming app, the widget is a js import and a simple html tag. Secured content simply fits as a div within the _slot_ of an html widget provided, such as

The import in the head of any protected html page.
```html
<script type="module" src="/our-signon-widget.bundled.js"></script>
```
The secured content in the body of the html page
```html
<body>
    <our-signon-widget>
        <div>
        ... secured content
        </div>
    </our-signon-widget>
</body>
```

As such, the Single Sign-On Widget can be integrated into various types of web applications, including those built with Thymeleaf or plain HTML. The integration process may vary, but the widget is flexible and can work with different frontend technologies.

## What if we want to change authentication providers such as from Firebase to Okta?

**Q: What if we want to change authentication providers such as from Firebase to Okta?**

A: Hey! We use Firebase because of it's generous free tier and copious companion offerings such as databases, storage, messaging and serverless offerings. These make it super easy to construct what we need for our single sign on infrastructure. 

But this is a very competitive market and there are many great options. 

Changing authentication providers is possible with the Single Sign-On Widget. You would need to update the configuration and authentication logic to accommodate the new provider. There are many including:

- Auth0
- Okta
- Amazon Cognito
- Azure Active Directory 
- OneLogin
- Ping Identity
- Keycloak
- Authenticator
- Stormpath

Offerings vary, but the common theme is the same. By farming out the most difficult part of security to specialists who have large teams fighting the bad guys and keeping things running.


## Can I change a student's role in one place and have it ripple to all API consumers?

**Q: Can I change a student's role in one place and have it ripple to all API consumers?**

A: The ability to change a student's role in one place and have it affect all API consumers is one of the biggest wins of this entire system.

This doesn't mean you won't have to maintain the APIs accordingly, such as when adding an "editor" role to an API where there was no such role before.

## Can I consume the widget as either regular HTML or a React component?

**Q: Can I consume the widget as either regular HTML or a React component?**

A: Yes, you can consume the Single Sign-On Widget in both regular HTML and React apps. 

Since React apps are written as [SPAs](https://en.wikipedia.org/wiki/Single-page_application), the sign on widget might best wrap the React app on the single containing HTML page.


This is how a typical React SPA might be secured. Note that most of this code is repeated from an example above.

The import in the head of any protected html page (as above)
```html
<script type="module" src="/our-signon-widget.bundled.js"></script>
```
The secured content in the body of the html page (as above except for React SPA root).
```html
<body>
    <our-signon-widget>
        <div id="root"></div> <!-- React SPA -->
        <script src="path_to_your_bundled_js_file.js"></script>
    </our-signon-widget>
</body>
```
As an alternative, [Lit allows a Web Component to be wrapped inside of a React component with relative ease](https://lit.dev/docs/frameworks/react/). So if it was necessary to secure _**only a part of your React SPA**_ with our Single Sign On Widget, no problem.

## Which app actually manages the roles for each user?

**Q: Which app actually manages the roles for each user?**

A: The management of user roles depends on your specific implementation. Typically, the app that requires role-based access control manages the roles for its users. The Single Sign-On Widget can help facilitate role-based authentication, but the role management logic may reside in the individual apps.

## What if authorization is identity-based in addition to or instead of role-based?

**Q: What if authorization is identity-based in addition to or instead of role-based?**

A: If you require identity-based authorization in addition to or instead of role-based authorization, you'll need to design your authentication and authorization logic accordingly. This may involve associating permissions with specific identities or user attributes.

## Code examples for standard API controllers and standard record-based security?

**Q: Code examples for standard API controllers and standard record-based security?**

A: Code examples for standard API controllers and record-based security can vary based on your technology stack. It's recommended to consult the documentation and best practices for your chosen framework (e.g., Spring Security, Django, etc.) to implement secure API controllers and record-based security.

## Do JWT custom claims have to anticipate the APIs that consume their roles?

**Q: Do JWT custom claims have to anticipate the APIs that consume their roles?**

A: Custom claims in JWTs (JSON Web Tokens) can be designed to anticipate the specific roles or permissions that APIs may consume. However, the exact design depends on your authentication and authorization requirements. It's important to align the claims with the expectations of the consuming APIs.

## What does the app have to do to consume the widget?

**Q: What does the app have to do to consume the widget?**

A: To consume the Single Sign-On Widget, the app needs to integrate the widget's components or libraries. This integration may involve configuring the widget with the required settings and handling authentication and authorization flows. The specific steps will vary depending on the technology stack and implementation.

## What does the API have to do to consume the JWT?

**Q: What does the API have to do to consume the JWT?**

A: APIs that consume JWTs need to implement authentication and authorization logic to validate and process the tokens. This typically involves verifying the token's signature, decoding its claims, and checking for required roles or permissions. The implementation details may vary based on the chosen framework and language.

## Can this be easily extended to apps and APIs not mentioned herein?

**Q: Can this be easily extended to apps and APIs not mentioned herein?**

A: The extensibility of this architecture depends on its design and flexibility. If the architecture is designed with modularity and scalability in mind, it should be possible to extend it to accommodate additional apps and APIs. However, the ease of extension may vary based on the specific implementation.

## Can these features be added incrementally instead of all in a big effort?

**Q: Can these features be added incrementally instead of all in a big effort?**

A: Yes, you can add features incrementally to the architecture rather than implementing everything in one big effort. This approach allows for gradual development and testing of new features, making it easier to manage and adapt the architecture over time.

## Will this architecture pass stringent modern-day security requirements?

**Q: Will this architecture pass stringent modern-day security requirements?**

A: The security of this architecture depends on its design, implementation, and adherence to best practices. To meet stringent modern-day security requirements, thorough security assessments and testing are essential. It's crucial to follow security standards and continuously monitor and update the architecture to address emerging threats and vulnerabilities.

## What if I want to use this in my own SSG like Next, Nuxt, 11ty, or Jekyll?

**Q: What if I want to use this in my own SSG like Next, Nuxt, 11ty, or Jekyll?**

A: You can integrate this architecture into your own Static Site Generator (SSG) like Next.js, Nuxt.js, 11ty, or Jekyll. The integration process may involve adapting the widget and authentication logic to work with the specific SSG technology you're using. It's possible to extend the architecture to support SSG applications.

## I'm confusing Authentication Providers with Sign-Ons Like Google/Facebook/Github

**Q: I'm confusing Authentication Providers with Sign-Ons Like Google/Facebook/Github**

A: Authentication providers and sign-ons like Google, Facebook, and Github can be related but serve different purposes. Authentication providers handle the verification of user identities, while sign-ons often provide convenient methods for users to authenticate using their existing accounts with popular services. You can use authentication providers in conjunction with sign-ons to enhance user authentication and authorization.

## How are bootcamp students harmed by tightly coupled security app/API combos?

**Q: How are bootcamp students harmed by tightly coupled security app/API combos?**

A: Tightly coupled security app/API combinations can pose challenges for bootcamp students as they may limit flexibility and make it more difficult to understand and modify security-related code. Students benefit from modular and well-documented security architectures that allow them to focus on learning specific concepts without unnecessary complexity.
