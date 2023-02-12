# How The Web Works For Aspiring Developers (Part I: URLs)

*Originally published on [https://frontendjoy.com/blog/how-the-web-works-for-aspiring-developers-part-one-urls](https://frontendjoy.com/blog/how-the-web-works-for-aspiring-developers-part-one-urls)*

Aria Tea sighs.

She has been browsing for <strong>4 hours</strong>, trying to understand <strong>how the Web works</strong>. She only found challenging, dull, or outdated content.

Are there only useless articles out there? Or worse, what if she needed a CS degree?

Slowly she loses her optimism 😢. Perhaps, she should renounce her dream to become a Web developer...

Does this feel familiar? Are you also a fresh developer with no clue how the Internet works despite many trials?

If so, keep reading.

In this series of articles, you'll learn :

- **What happens when you visit a Website**
- **How the Internet works, and the difference between the Internet and the World Wide Web (WWB)**
- **What your role is as a Web Developer**
- **[Bonus] How to create your website and make it available online 🙂**

![sad girl learning how the web works](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fsad_girl.webp&w=1200&q=75)

12:00 PM: it is break time, and you need to check Twitter. You go to your favorite **browser** (ex: Chrome, Safari) and type the text [https://twitter.com](https://www.twitter.com/), also called an **URL**.

# What's an URL?

Imagine you're at a **Google career fair**.

You want to know how to become a **Developer** in the **Android Team** located in **Mountain View**.

After **1h of hunting**, you've found a recruiter from the team. Her name is **Sansa** and ... she loves your story.

Unfortunately, you didn't bring your resume + cover letter. So, she asks you to send your application by **postal mail** (I know that's unlikely but bear with me 😅).

The next day, you rush to the post office. Once there, you'll make an **order** with the following information :

- **The sending method:** Are you adding tracking? Do you need the recipient's signature? Etc.
- **The Mountain View office address** - 1600 Amphitheatre Pkwy, Mountain View, CA 94043
- **The Engineering building number** - N°123 (fictional number)
- **The team name** - Android
- **The recruiter name** (optional) - Sansa

The postal service can then deliver your mail 📩.


![application letter to Google](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fletter_to_job.webp&w=1200&q=75)

Like with letters, you need to **provide some information to your browser** before it can access a Web resource (i.e., any resource available over the Web).
You're achieving this through the **URL** (**Uniform Resource Locator**), the text present in the browser **navigation bar**.

When you enter an URL in your browser, you're giving it the location of a resource + how to access it.

![examples of urls](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexamples_of_urls.webp&w=1200&q=75)


# What makes up an URL?

Like shipping orders, an URL contains several components :

## 1. The scheme

![examples of schemes](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_of_schemes.webp&w=1200&q=75)


The scheme is to the URL what the sending method is to your shipping order.
It **defines how the browser will access the resource**.
Will the connection be secure? Will it be a file transfer? Etc.

Different values are possible :

- `HTTP`: for unsecured connections
- `HTTPS`: for secure connections (it's like sending an encrypted mail + recipient signature).
- `mailto`: for sending emails
- Etc.

We also say that the scheme specifies the **protocol** used: HTTP identifies the <em>**HyperText Transfer Protocol**</em>, HTTPS identifies the <em>**HyperText Transfer Protocol Secure**</em>, etc.

You can think of a protocol as a **language but for computers**.

- People - from different countries (e.g., France, US) - can communicate when using the same language (e.g., English).
- Computers - on different systems (e.g., Apple & Microsoft) - can communicate using the same protocol (e.g., HTTPs).

**We'll learn more about some of the protocols later on.**

> 💡 When using HTTP or HTTPs as a scheme, you don't need to add it to the URL: the
> browser will resolve the correct one.
> Ex: www.google.com and https://www.google.com/ lead to the same page.
>
> Think of it like going to the post and not specifying a sending method: the agent will assume the default one.

## 2. The domain name

![examples domain names](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_domain_names.webp&w=1200&q=75)

The domain name is to the URL what the Mountain View office address is to the shipping order.
It's **unique to the website**.
In the same way, you can't find another company at the Google campus address, you can't find different websites with the same domain name.

Like real-world addresses, **a domain name contains many parts**, separated by a dot:

- **The top-level domain or TLD**
  ![examples top level domains](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_tlds.webp&w=1200&q=75)
  
  It is the last part of the domain name. You have to pick among a **fixed list of
  TLDs** maintained [here](https://data.iana.org/TLD/tlds-alpha-by-domain.txt). Some
  are available to everyone while others aren't. A few examples :

  - `Com` => anyone can use it;
  - `Gov` => only US government entities can use it;
  - `Edu` => only available to [higher](https://en.wikipedia.org/wiki/Higher_education) education institutions;

  You can learn more about the restrictions [here](https://en.wikipedia.org/wiki/List_of_Internet_top-level_domains#Country_code_top-level_domains).

- **The second-level domain or SLD**
  
  ![examples second-level domains](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_slds.webp&w=1200&q=75)

  It comes before the TLD and **usually corresponds to the website name**.
  You're free to pick any name with some exceptions.
  For example, only France airports can use `.aeroport.fr` (ex: [http://www.toulouse.aeroport.fr/](http://www.toulouse.aeroport.fr/))

- **The subdomain (sometimes referred to as the third-level domain)**

  ![examples subdomains](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_subdomains.webp&w=1200&q=75)

  It comes before the SLD. `www` is the most frequent one.
  Hence, if you don't specify a subdomain, your browser will assume `www`.
  For example, https://www.freecodecamp.org/ and https://freecodecamp.org/ lead to the same page.

  Subdomains help **organize online content**. For example, the firm **tailwind** uses :

  - [https://tailwindcss.com/](https://tailwindcss.com/) for its main content,
  - [https://blog.tailwindcss.com/](https://blog.tailwindcss.com/) for blogging,
  - [https://play.tailwindcss.com/](https://play.tailwindcss.com/) for experimentation,
  - Etc.

  You may wonder, why not use URLs like www.blogtailwind.com or www.tailwind.com/blog?

  Two main reasons :

  - **You have to pay for a domain, not for a subdomain** - For your website to use a domain name, you have to pay for it.
    You don't have to for a subdomain: you can generally create one for free.
  - **Your subdomain is like a website of its own** - Think of it as a **parcel house**.
    You can manage it separately from your main website.

  > 💡 There may be **more than 3 levels** in the domain name: fourth-level, fifth-level, etc.
  >
  > For example, in https://www.findajob.dwp.gov.uk/ :
  >
  > - <strong>
  >     <em>TLD</em>
  >   </strong> = uk
  > - <strong>
  >     <em>SLD</em>
  >   </strong> = gov
  > - <strong>
  >     <em>Third-level domain</em>
  >   </strong> = dwp
  > - <strong>
  >     <em>Fourth-level domain (or subdomain)</em>
  >   </strong> = findajob

## 3. The port (optional)

![examples ports](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_ports.webp&w=1200&q=75)

Imagine all the packages coming to the Google campus. Each one must go to the correct building.

Your computer is **like the Google campus**. It **receives several Internet messages, and each must go to the proper application**.
For example, if you receive a Skype call, it must go to the Skype app; if you receive a mail, it must go to the mail app, etc.

Your computer needs to manage that traffic, and it does so using **ports**.
Each port has a **unique number** called a **port number**.
Two different applications can't use the same port. When an Internet
message comes with a port number, only the application using that port receives
it. We say that **the application is "listening" on that port**.

There are **+60,000 ports** on your computer, but **only some are open** (i.e., free to use) for security reasons.
In the same way, you don't open all the doors of your house, you don't open all the ports on your computer 🙂.

> 💡 By default, **computers listens on ports 80 and 443 for Web communication**, so there is no need to add it to the URL (the browser can even remove it).
>
> - 80 is used for HTTP messages
> - 443 is used for HTTPs messages

## 4. The path

![examples paths](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_paths.webp&w=1200&q=75)

Once your parcel is in the correct building, it must go to the **proper team**.

Once you tell your browser which website to access, you need to tell it **which specific resource you want**.

In fact, websites generally contain many pages/resources. Each one is identified by a text called the **path**.

> 💡 When there is no path in the URL, you'll land on the default website page.

## 5. Some (optional) parameters

![examples parameters](https://frontendjoy.com/_next/image?url=%2Fstatic%2Fimages%2Fhow-the-web-works-for-aspiring-developers-urls%2Fexample_parameters.webp&w=1200&q=75)

Your parcel finally reached the Android team 🎉. If you specified **Sansa** (the recruiter's name) on it, it will go to her, else to the whole team.
In the last case, you run the risk of having your application rejected/handled by the wrong person.

By analogy, **URL parameters carry extra information**. Sometimes they're optional, sometimes required.
They're **located after the interrogation point `?`**. When there is more than one, they're **separated by `&`**.
Here are a few examples :

- [https://www.youtube.com/watch?v=PkZNo7MFNFg](https://www.youtube.com/watch?v=PkZNo7MFNFg) - `v` is **mandatory** and identifies the video.
- [https://ahrefs.com/blog/?s=subdomain](https://ahrefs.com/blog/?s=subdomain) - `s` is **optional** and only used for search

> 💡 The URL may contain an extra part called the **anchor**. It links to a section of the page
> , so the user doesn't have to scroll.
>
> For example, in [https://frontendjoy.com/blog/how-the-web-works-for-aspiring-developers-part-one-urls#conclusion](https://frontendjoy.com/blog/how-the-web-works-for-aspiring-developers-part-one-urls#conclusion), the anchor is **conclusion**.

# Conclusion

If you're like Aria Tea, you still don't understand how the Web works at this point 😅. You may have questions like :

- What happens after I pasted the URL?
- Where is the website content located?
- Etc.

We'll answer those questions in the next articles 🙂.