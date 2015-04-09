---
layout: page
title: "Projects"
description: "Personal Projects"
group: navigation
---
{% include JB/setup %}

### [Codag](http://codag.ch)

I'm the owner of Code Agency Juchli (Codag).

Codag provides agile Software Engineering with a main focus on Web development, Restful APIs, Messaging.
We rely on OpenSource in our daily business. To give the community something back, we from time to time open source some of our in-house developed projects (see [Codag@Github](https://github.com/Codag)) or contribute to other projects directly.

Besides, with interests in server administration, service quality and availability as well as the open source project [ISPConfig](http://ispconfig.org), we are maintaining a powerful hosting environment:

- Nameservers
- Mailservers
- Webservers
- VPS Servers
- Shared Hosting

We are specialized in providing individual hosting setups. Especially ambitious clients using Symfony2 appreciate a harmonized server/application environment.
<br /><br />

* * *

### Haskell Message Broker (HMB) <small>(Bachelors Thesis)</small>

Work in progress: [https://github.com/hmb-ba](https://github.com/hmb-ba)

<br /> <br />

* * *

### Smart Meeting Planner <small>(Studienarbeit)</small>

<b>Thesis:</b> <a href="../uploads/Thesis_Studienarbeit_SMP.pdf">PDF Download</a><br>
<b>Project Partner:</b> Flughafen Zürich AG

Die Organisation von Meetings in grösserem Geschäftsumfeld, insbesondere beim Flughafen Zü-
rich, gestaltet sich als zunehmend komplizierter und wird durch die bestehenden Mail- und
Kalendersysteme noch zu wenig gut unterstützt. Vielfach müssen dafür verschiedene Dienste wie
z.B. der Microsoft Exchange Kalender, Doodle Pools und klassische Mails/Anrufe kombiniert
eingesetzt werden, um den Prozess der Terminfindung bis zur Termineinladung zu meistern.
Dies liegt daran, dass die einzelnen Dienste nur spezifische Aspekte für sich besonders gut un-
terstützen (z.B. gemeinsamer Kalender, Termin-Umfrage).
<br><br>
Im konkreten Fall von Microsoft Exchange Server mit Outlook Clients ist zwar gemeinsame
Termineinladung und Einsicht in die Ressourcen (Kalender von Personen und Räumen) gut
unterstützt. Jedoch fehlt ein effizienter Mechanismus, um freie Terminvorschläge zwischen be-
teiligten Personen mit entsprechenden freien Räumlichkeiten zu eruieren. Dies lässt sich durch
Einsatz einer externen Termin-Umfrage (wie z.B. Doodle) bewältigen, wobei dies jedoch in vielen
Unternehmungen aus Gründen von Geheimhaltung und Datenschutz vermieden wird.
<br><br>
Das Ziel dieser Studienarbeit ist es deshalb, einen Prozess zur verbesserten intelligente Termin-
findung zu konzipieren. Dieser soll dann als Prototyp mit Integration in den Microsoft Exchange
Servers (allenfalls mit Outlook) implementiert werden.

<br /> <br />

* * *

### Issue Bidder

<a href="/assets/img/issuebidder_profile.png"><img src="/assets/img/issuebidder_profile.png" width="90%" /></a>

Since we live in a fast, growing and busy world you don't have always time to fix important issues and you don't have always time to implement new things on existing systems. That's okay and that's just normal - even though they would be important. Here comes Issue Bidder in the game. We let you post an Issue created on GitHub so that not only you but also any other people they are interested in that issue, can offer any amount of money in the hope that an other developer can fix this.
<br><br>
The application is built with Symfony2 and makes among other things extensive use of the following API's:
<ul>
    <li>GitHub</li>
    <li>PayPal</li>
</ul>
The server setup is based on a personal created VPS server.
A NGINX webserver with a tailored configuration and SSL certificate brings the necessary performance and security.
<br><br>
<b>Link: <a target="_blank" href="https://issue-bidder.com">www.issue-bidder.com</a></b>
<br /> <br />

* * *

### VKweb

<a href="/assets/img/vkweb_einsatzverwaltung.png"><img src="/assets/img/vkweb_einsatzverwaltung.png" wdith="80%" /></a>

**A management and planing tool for the Verkehrskadetten Zürichsee.**

VKweb initially was a is a school project (Software Engineering Project) together with 3 team members. As this is a Symfony2 project on server-side my role is also to lead the team in terms of technical decisions and the procedure of development.
<br><br>
As the customer was planing each event using multiple excel sheets the goal is to centralize each process into one application. This will prevent the customer redundancy and significantly decrease the amount of mistakes during the process.
Consequently this lets the customer plan an event more efficient.
<br><br>
Technologies:
<ul>
    <li>Server-Side: RESTful API using Symfony2</li>
    <li>Client-Side: AngularJS</li>
    <li>Projectmanagement: Atlassian JIRA/Confluence</li>
</ul>
<br>
<b>Project Information: <a target="_blank" href="http://vkweb.codag.ch">vkweb.codag.ch</a></b>