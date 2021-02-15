# Shipping High Quality JS Apps with Confidence with Tomasz Łakomy
### *[Test JS Summit](https://testjssummit.com) - January 28-29, 2021*

@TestJSSummit #TestJSSummit

Tomasz Łakomy
- https://twitter.com/tlakomy
- https://github.com/tlakomy
- https://tlakomy.com/

[My tweet thread on the presentation.](https://twitter.com/aaron_stroud/status/1354871243263012866)

<hr />

There's no one *silver bullet* to improve our testing.

> "The more your tests resemble the way your software is used, the more confident they can give you." —Kent C. Dodds

A better way to think of it is a Testing spectrum:
Developer <--> User

Starting with the Developer:

**Prettier**
- Helps us reduce the *Minimize editor -> browser -> editor* cycles

**TypeScript**
- Use TypeScript for project Medium-sized and up.
- Static types eliminate a certain type of bug
- Helps us reduce the *Minimize editor -> browser -> editor* cycles
- recommend, only use TS in strict mode
- Remember—Type safety !== Bug free
	- Type safety means it doesn't have any type issues
- Easy to use TS for greenfield [new projects], difficult for legacy [projects]

**React Testing Library**
- Using React Testing Library can assist you creating accessible apps
- Use [Testing-Playground.com](Testing-Playground.com) by [@meijer_s](https://twitter.com/meijer_s)
- Recommend: [Building Accessible Web Apps with React Workshop Prep](https://egghead.io/playlists/building-accessible-web-apps-with-react-9bd35e9e) by [Erin Doyle](https://twitter.com/SunshinyDoyle) (first part of course is free)

**Cypress**
- End to End testing
- His favorite tool and he feels its fun
- [Cypress Testing Library](https://github.com/testing-library/cypress-testing-library)
	- you'll write better queries
	- easier to shift from the React Testing Library (types of tests) to end to end
	- note from discord chat: "Cypress testing library is a way to use cypress with the idiom of testing library"


Getting closer to the user now.

Speed is vital for the user's experience.

**DataDog**
- Dashboards to monitor performance
- Troubleshoot and resolve the javascript errors in production

Using your own product–Dogfooding–is the ultimate form of testing.

**Testing in production**
- Staging environment is still very different than production
- Corey Quinn @QuinnyPig calls his "staging environment 'Theory' on account of how many things work in theory, but not in production."
- Recommends: LaunchDarkly
	- Different feature flags to test new features–in production–on a small percentage of the user test, gradually scaling up.
	- Good for A/B testing too

High quality stems from fast feedback loops.

Develop -> Ship -> Feedback -> Repeat

Developing high quality code is always faster than fixing production at 2am.

> "Overall, the best way to reduce bugs is to make software simpler" —John Ousterhout, *A Philosophy of Software Design*










There's no one silver bullet to improve our testing.
A better way to think of it is a Testing spectrum:
Developer <--> User
#TestJSSummit


Tools like Prettier & TypeScript help us reduce the
*Minimize editor -> browser -> editor* cycles
#TestJSSummit


High quality stems from fast feedback loops.
Develop -> Ship -> Feedback -> Repeat
#TestJSSummit


Recommends:
Testing-Playground.com @meijer_s
Building Accessible Web Apps with React Workshop Prep @SunshinyDoyle
https://egghead.io/playlists/building-accessible-web-apps-with-react-9bd35e9e
#TestJSSummit

Using your own product–Dogfooding–is the ultimate form of testing.
Testing in production is important because the staging environment is still very different than production.
#TestJSSummit


Developing high quality code is always faster than fixing production at 2am.
"Overall, the best way to reduce bugs is to make software simpler" —John Ousterhout, A Philosophy of Software Design
#TestJSSummit