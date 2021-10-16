## Tweekr's progress so far

## Idea

Got the idea on October 6th, 2020. I was going through my Twitter bookmarks and found an opportunity for a product there. I bookmarked a lot of tweets but never came back to them. Once in a blue moon when I'd remember that a bookmarked tweet could solve a problem I had, it would take me forever to find it in the bottomless list of bookmarked tweets that I had.
This was when I started thinking hard about building Tweekr.

## Started building

It took me a month to get myself to writing code because I was mostly researching the Tech stack and all that jazz. Here lies my first mistake. Now I know that, if there's something you're building, build an MVP as fast as possible, get it in front of people, get feedback, and only then, iterate on the MVP and refine your product if it adds value. But I waited for a month. Researching for the perfect tech stack. This is my first actual product and I had no clue about how products are built, how an MVP is built, and how important feedback is.

## November flies by

November was spent worrying about how I could minimize Firestore reads and writes so that I don't end up paying a huge bill to Firebase. And boy was this month tough. I was thinking about Tweekr all the time. Almost got burnt out without even writing much code. Because all I was doing mostly was research.

## December

One good thing I did was to launch the waiting list and talk to people on Twitter if they'd actually be willing to give Tweekr a spin. 

%[https://twitter.com/thisismanaswini/status/1337430119938289665?s=20]

I asked them about things they'd like to see etc... And I grew so tired of my inability to build consistently yet worry perpetually. So I tweeted this

%[https://twitter.com/thisismanaswini/status/1339967939911380995]

This time there was a public commitment involved and I worked extremely hard from 15th December to 26th December(Because I failed to release Tweekr on 22nd :(  ) and then came the time to deploy the app.

Now let me tell you a story!

I sat down at 7 pm to deploy Tweekr. It was still using Firestore as the database but I postponed worrying about it till later in the future. I tried to deploy it to Vercel, but it showed a consistent error for an HOUR! And while deploying, I think, since it is my first product, I was determined to just get done with Tweekr as soon as possible. So I skipped from Vercel to Firebase's deployment. I deployed after a lot of hard work. But, the result was not what I expected it to be.

I opened the app on my mobile and my heart sank! It was not responsive at all. I took great pains while developing, to make sure it was responsive. I guess I did something terribly wrong in the stylesheets. By this time, it was 10 pm and I lost all hope.

The next thing I did, might sound very foolish but it is the reason I'm still sane today, while building Tweekr, and that is, to abandon Firebase as the backend and code a custom backend with a PostgreSQL database this time.

I'll write more about why I changed my Tech stack and all those technical details in another post in this series.

This tweet shows my progress, before I abandoned started coding a custom backend.

%[https://twitter.com/thisismanaswini/status/1341401044652322818?s=20]

## Change in the Tech stack and new ideas

This was the beginning of January 2021 and I was filled with a fresh enthusiasm to build Tweekr ASAP and get it in front of its users. But this time, something changed inside me. Instead of an "Eternal worry and zero code" approach, I took a more mature take. I told myself "No matter the mistakes I commit, I need to believe that my future self will be intelligent enough to handle them. So all that I do now is code. But before that, I need to figure out why EXACTLY I'm making this and only include those features in the MVP, that satisfy those small number of needs". 

This was the time I joined communities like GenZMafia and became active on Discord. I met awesome people there and they helped me arrive at what Tweekr today is, in my head. Rethinking all of it in terms of a Browser extension + Web app. 

## Livestreaming the process

I built the entire Browser extension over a course of 7 consecutive days while live-streaming on Twitch. And then, again, fell off track due to exams. 

## Now, its February 7th i.e. TODAY

Today I've realized that I delayed this enough and I'm very very determined to finish building the MVP in Feb and release it to a small group of initial users. Today I was talking to a friend on Twitter and he told me about the YC Build Sprint happening from Feb 15th - Mar 14th. 

I took the challenge up right away. So now, I have 9 days before the sprint begins. Till then, I'll mostly be building Tweekr still. After that, during the sprint, I'll finish up the MVP before Feb ends and will release it as a beta. That's the plan for now. 

## Conclusion

This time at least, I want to make sure I set reasonable goals, stop over-optimizing code, stop brooding over the perfect tech stack and just build the most important features and cook up a decent MVP for release!

That's it then. This was an impromptu post about Tweekr and why it is taking me almost forever to release it.