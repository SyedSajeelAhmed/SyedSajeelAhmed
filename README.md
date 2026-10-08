# Syed Sajeel Ahmed — Developer Portfolio

A cinematic, 3D personal portfolio built with ASP.NET Core MVC. It presents my professional experience, technical skills and project work as a single scroll-driven journey, with a working contact form that delivers messages straight to my inbox.

![ASP.NET Core](https://img.shields.io/badge/ASP.NET_Core-MVC-512BD4?style=flat-square&logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?style=flat-square&logo=csharp&logoColor=white)
![three.js](https://img.shields.io/badge/three.js-r128-000000?style=flat-square&logo=threedotjs&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-F7DF1E?style=flat-square&logo=javascript&logoColor=black)
![MailKit](https://img.shields.io/badge/Email-MailKit-0A7EA4?style=flat-square)

![Portfolio home page](docs/hero.png)

---

## Overview

The site is a single-page experience rendered by one Razor view. A WebGL scene moves the camera forward as the visitor scrolls, and a cut-out figure of me travels through each section: About, Skills, Experience, Major Project, Projects and Contact. All interface content on the hero is live HTML positioned over a backdrop image, so text stays crisp, selectable and accessible at every screen size.

**Live projects featured on the site**

| Project | Description | Link |
|---|---|---|
| Restaurant ERP | Multi-branch, multi-store restaurant ERP with POS, accounting and inventory modules | Demo on request |
| Fitsylo | Fitness platform I founded and develop | [fitsylo.bsite.net](https://fitsylo.bsite.net/) |
| Travel Management System | Travel and transit booking platform | [blessedjourney.vercel.app](https://blessedjourney.vercel.app/) |
| Web Reporting System | Web-based business reporting and dashboards | [webreporting.devaj.technology](https://webreporting.devaj.technology/) |
| Food Ordering System | Online ordering platform for restaurants | [order.devaj.technology](https://order.devaj.technology/) |

---

## Features

**Experience**
- Scroll-driven 3D corridor built with three.js, with a distinct zone for each section
- Animated character that follows the visitor through the page and turns toward the mouse
- Mouse parallax across multiple depth layers, magnetic buttons and a custom cursor
- Smooth scrolling with Lenis
- Interactive skills orbit with descriptions on hover or tap

**Projects**
- Project cards and detail views generated from a single data file
- Hash-routed detail view (`/#project/<slug>`) with shareable links and browser Back support
- Screenshot gallery with keyboard navigation, swipe, thumbnails and a full-size lightbox
- Muted, looping video walkthroughs with MP4 and WebM sources
- "Request Demo" and "Request Details" actions that open the contact form with the subject pre-filled

**Contact**
- Server-side email delivery through MailKit over SMTP
- Anti-forgery token validation on every submission
- Honeypot field for basic spam protection
- Automatic reply to the sender, with Reply-To set to the sender's address

**Quality**
- Responsive layouts for desktop, tablet and phone, with lighter effects on smaller devices
- Respects `prefers-reduced-motion`
- No external CDN calls: three.js, Lenis and all fonts are served locally

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | ASP.NET Core MVC, C# |
| Views | Razor |
| 3D and animation | three.js (r128), Lenis 1.1.13, vanilla JavaScript |
| Styling | Custom CSS with design tokens (no CSS framework) |
| Email | MailKit and MimeKit |
| Fonts | Sora, Manrope, JetBrains Mono (self-hosted) |

---

## Project Structure

```
SajeelPortfolio/
├── Controllers/
│   └── HomeController.cs          Index page and SendMail endpoint
├── Models/
│   └── ContactFormModel.cs        Contact form payload and validation
├── Services/
│   └── EmailService.cs            SMTP settings, MailKit delivery and auto-reply
├── Views/
│   └── Home/
│       └── Index.cshtml           The complete single-page site
├── wwwroot/
│   ├── css/portfolio3d.css        All styles
│   ├── js/
│   │   ├── portfolio3d.js         3D scene, scroll choreography, hero layout
│   │   ├── projects-data.js       Project content (one object per project)
│   │   └── projects.js            Project cards and detail view
│   ├── images/                    Hero backdrop, character and project screenshots
│   ├── lib/                       three.js and Lenis
│   └── fonts/                     Self-hosted web fonts
├── Program.cs
└── appsettings.json
```

---

## Getting Started

### Prerequisites

- [.NET SDK](https://dotnet.microsoft.com/download) matching the project's target framework
- An SMTP account for the contact form (Gmail with an App Password works well)

### Installation

```bash
git clone https://github.com/<your-username>/SajeelPortfolio.git
cd SajeelPortfolio
dotnet restore
```

### Configure email

Add the email settings to `appsettings.json`. Leave the password empty here; it should never be committed.

```json
"EmailSettings": {
  "Host": "smtp.gmail.com",
  "Port": 587,
  "UserName": "you@example.com",
  "Password": "",
  "FromAddress": "you@example.com",
  "FromName": "Portfolio Contact Form",
  "ToAddress": "you@example.com",
  "SendAutoReply": true
}
```

Store the password with .NET user secrets during development:

```bash
dotnet user-secrets init
dotnet user-secrets set "EmailSettings:Password" "your-app-password"
```

In production, set it as an environment variable on the host instead:

```
EmailSettings__Password=your-app-password
```

Make sure the service is registered in `Program.cs`:

```csharp
builder.Services.Configure<EmailSettings>(builder.Configuration.GetSection("EmailSettings"));
builder.Services.AddScoped<IEmailService, EmailService>();
```

### Run

```bash
dotnet run
```

Then open the URL shown in the console.

---

## Managing Content

**Projects.** Every project lives in `wwwroot/js/projects-data.js`. Adding or editing a project means editing only that file; the cards and detail views are generated from it.

To add screenshots to a project:
1. Place images in `wwwroot/images/projects/<slug>/` as `01.webp`, `01-thumb.webp`, `02.webp` and so on.
2. Reference them in that project's `images` array in `projects-data.js`.

To add a video, follow the Food Ordering entry. Videos play muted, so strip the audio and compress before adding:

```bash
ffmpeg -i input.mp4 -an -vf scale=1280:-2 -c:v libx264 -crf 30 -preset slow -movflags +faststart demo.mp4
```

Projects with no link, screenshots or video automatically show a "Request Details" button that leads to the contact form.

**Skills, experience and about.** These sections are edited directly in `Views/Home/Index.cshtml`.

---

## Security Notes

- Credentials are read from user secrets or environment variables, never from source control.
- The contact endpoint validates an anti-forgery token and rejects invalid models.
- A hidden honeypot field filters out simple automated submissions.

---

## Contact

**Syed Sajeel Ahmed**
ASP.NET Developer and Software Engineer, Karachi, Pakistan

- Email: [syedsajeelahmed0@gmail.com](mailto:syedsajeelahmed0@gmail.com)
- LinkedIn: [linkedin.com/in/sajeel-ahmed](https://www.linkedin.com/in/sajeel-ahmed-03ba88303)
- Fitsylo: [fitsylo.bsite.net](https://fitsylo.bsite.net/)

---

## License

All rights reserved. The source code is shared for viewing and reference. The personal content, images and project screenshots may not be reused without permission.
