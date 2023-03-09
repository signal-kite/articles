# 25 things to check when preparing a landing page for a launch

**Warning! This article may have opinions you don't agree with.**

Before you launch your landing page, take time to check all these things. Some of them look pretty minor and unsignificant but can impact the effect your LP will have and it's not only about amount your visitors. The landing page should:

- have nice, catchy design and graphics
- have clear, catchy text
- make people do something (so called CTA, or call to action)
- make people bookmark the website (to come to it later if they are not interested in it now)
- be good for search systems (SEO).

We split all these things to check into groups by topic but keep in mind, some of them can be part of other group.

## 1. Design

### 1. Graphics
Do not use free graphics like Undraw or Absurd illustrations. Not only they are very overused (what makes your website looking as thousands of others) but they are often misleading, distracting, and taste-less. 

Don not use free photos you can find on Unsplash. This is a very good resource but many photos have already been used thousand times what again, makes your website look amateurish.

You can hire a freelance photographer or illustrator, for example here: https://www.freelancer.com/ We found this resource pretty affordable and the results satisfying.

Another source of great free illustrations (that are actually not free) is [Adobe Stock](https://stock.adobe.com/). The trick is to use their free trial that allows you to download 10 free resources. Be careful though, not all of them are great (we found "seamless patterns" not seamless at all, for example).

A couple of other sources of free graphics:
- [LapaNinja free illustrations](https://www.lapa.ninja/freebies/illustrations/)
- [Storyset](https://storyset.com/)

### 2. Colors
There pretty common principles from the leading designers that will help you with coloring even if you (like us) are not designer al all:

- do not use very contrast colors together (like yellow and green, red and blue, and so on)
- do not use low contrast colors together (like light grey on light black)

#### 2.1. How to choose a palette
When selecting a palette, pick up 1 (max 2) main colors, and 1 or 2 additional (usually they are white or grey). But if you picked 2 main colors, use one as main (for background or big elements) and the second one - for small elements (underlining, small dots, or the first capital letters.

To find the colors that will go toghether well, use the color wheel. It can be found, for example, here: https://en.wikipedia.org/wiki/Color_wheel

<img src="https://user-images.githubusercontent.com/125164513/221445007-414f91ac-548e-4bd7-b1b2-3d5dc10aff9d.png" width="300"/>

The idea is very simple: pick up 2 colors that are on the opposite sides of wheels and you will be good. For example:

- aqua and yellow
- violet and green
- light blue and orange.

Of course, it's not necessarily to follow strict colors, but the tints could be selected in the same way.

### 3. Main design
Do not copy the design of other websites, especially popular ones. First of all, your website will look the same, secondly, sometimes the design is pretty controversial and copying is always too obvious what makes bad impression. The example: [Gumroad website](https://gumroad.com/). If you try to copy it, your website could look just a poor copy.
Compare:
<img src="https://user-images.githubusercontent.com/125164513/221445546-224e2012-d3db-40f5-a68e-2dffb35d1086.png" width="500"/>

and 
<img src="https://user-images.githubusercontent.com/125164513/221445686-e50abf0a-7df7-4d65-aa4b-fe7cbf3f5946.png" width="500"/>

**Don't do that!**
But what you can do is to borrow the ideas, layout, designs. "Borrow" means you do not blatantly copy but just use the ideas and apply to your own website.
2 great sources of inspirations: [Lapa Ninja](https://www.lapa.ninja) and [Landingfolio](https://www.landingfolio.com/).

#### 4. Logo
If it's your first project it's totally fine to have a simple, homemade logo. Some ideas on how to create it:

- hire a freelancer on https://www.freelancer.com/
- use combination of letters
- use icons
- ask [Midjourney](https://www.midjourney.com/) to generate it.

#### 5. Small elements
Icons, dots, arrows and other small elements make the website bright and lively. But don't overuse it. 
We found these free icon sets the best choice:
- [Tabler Icons](https://tabler-icons.io/) - 3400 icons
- [FontAwesome](https://fontawesome.com/) - 2020 icons.

Also frameworks, like [Bootstrap](https://getbootstrap.com/) - 1800 icons have their own icon sets.

#### 6. Background
Can be anything: an image, pattern, or just a color. Just remember that it should not distract the user's attention. 

#### 7. Fonts
The main designers' rule is: no more than 2 different font families and no more than 4 different styles totally.
You can find free and easy-to-use fonts at [Google Fonts](https://fonts.google.com/).

#### 8. Using frameworks
Frameworks are great but don't forget to change the design. We saw so many similar-looking websites that would look great if they would be different.
Frameworks not only help make the website development easier and fater but also help with responsiveness.
Some popular frameworks:
- [Bootstrap](https://getbootstrap.com/)
- [Bulma](https://bulma.io/)
- [TailwindCSS](https://tailwindcss.com/)

The big list of great frameworks can be found here: https://github.com/troxler/awesome-css-frameworks

#### 9. Responsiveness
Nowadays, the mobile devices are used for browsing the Internet. So, the responsiveness is crucially important for websites.
Usually, it can be done with [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) but web frameworks already solved this problem for you so you don't need to (see the list in the previous point). 
The modern browsers allow checking your website on small screens, for example, how it can be done on Chrome:

![image](https://user-images.githubusercontent.com/125164513/221448904-c282dcd5-a6b0-4e17-8632-aa2f70dfc99e.png)

**Do not forget to check all the elements like buttons, links, menues.**

#### 10. Favicon
Many experts think you can launch without a favicon. Sure, you can but trust us, a website without a favicon looks so amateurish that it can affect the results of launch.
##### Q&A
##### What is a favicon
It's a small picture shown on the browser's tab:
![image](https://user-images.githubusercontent.com/125164513/223312093-6d222b15-3f17-4449-bd48-85da8c40d3c6.png)

##### What size is that?
It should be 16x16.

##### Which extension is the file?
Actually, it can be anything: .ico, .png, .jpg and so on but for best compatibility use .ico.

##### How to make it?
Usually it's a smaller copy of your logo. Create a square logo and resize it into your favorite editor. Then save it as .ico. If your editor can't save a graphical file as .ico, use any online tool, for example, this free popular tool [CloudConvert](https://cloudconvert.com/png-to-ico).

##### Where to put it?
You can put it anywhere in your website but for the best results, copy it into the root folder. The file should be called favicon.ico.

##### How to insert it?
In your HTML file add the following code (inside the "&lt;head&gt;" tag):
```
<link rel="icon" type="image/x-icon" href="/favicon.ico" />
```
## 2. Content

#### 11. Hero section
There are many articles explaining what to put there but we like [this comprehensive guide by Julian Shapiro](https://www.julian.com/guide/startup/landing-pages).
Shortly speaking, the header should describe what your product does and the subheader should give the idea on how.

_Tip: Use ChatGPT to proofread and even generate good hero texts!_

#### 12. CTA
CTA or "call-to-action" is a button or link prompting a user do something right now. For example, it may be a link to the registration page, or a form to leave their email (we discuss this form in the Functionality section). It's a good idea to put several buttons or forms on the page but do not overuse it.

#### 13. Features? No!
We see so many landing pages that are focused on their features that we often loose the whole purpose of the service or tool. Instead, it's always better to explain how your product makes the users' lives easier. Which does it solve? how effectively? faster? 
To make the message clear you need to understand which users you are targeted to and solve _their_ problems. Always ask "why" until you gain the core problem to be solved.
##### The example of asking "why"
A web tool helps with memorizing words in a foreign language. One of the features they described was "easy-to-use innovative flash cards" what said nothing to us. Do we really need an "innovative" flash cards? How does it help? We started asking "why".
1. Why do wee need the innovative flash cards? - They helps you to memorize words 35-55% faster.
2. Is this number significant? - Yes, you will spend a half less time to memorize words, or you will be able to memorize twice words for the same time.
3. Do we need to memorize them faster? - Yes, because you have many other things to memorize and prepare.
4. Why? - Because you are preparing to the internation certificate exam (the target audience were student preparing for these exams).
5. Why do we take the exam? - Because you want to move to another country.
6. Why? - Because you want a better quality of life for you and your family or/and new experience and new friends (let's focus on the second group).
7. Now, we can change the feature from "easy-to-use innovative flash cards" to "innovative flash cards "


## 3. SEO and social media

## 4. Functionality

#### Email form
Always prompt a user to do something. If you have only a landing and no product yet, you usually wants them to leave their email. You can program it on your own (if you can code, it's a pretty easy task - for example, you can add a simple server code that just sends you email with the user's email) or use a third-party service. We tried several and our latest choice is [Sender](https://www.sender.net/). Their pricing is nice and their support is great and fast (if you really need it).

#### Survey
Another popular form of validation the idea or collecting opinions are surveys. The simple, effective and free tool is Google Forms.
##### Some tips
1. But don't overuse it! Do not ask too many questions and don't ask personal questions (like phone number and name) if you really don't need them.
2. If you collect email/name don't make these fields mandatory.

## 4. All the minor things that are worth to check
##### Broken links
Check all the links you have on your page (they can be found everywhere but I often see then in the footer). Sometimes founders would like to add stubs leading to the same page (like links "About" or "How it works") but I wouldn't do that - it makes the website less truthful.

##### Page title
