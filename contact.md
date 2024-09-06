---
layout: default
title: Contact
---

<div id="contact">
  <h1 class="pageTitle">Mes coordonnées</h1>
  <div class="profile-photo">
	<img src="{{ '/assets/img/pp.png' | relative_url }}" alt="Photo de Profil" />
  </div>
  <div class="contactContent">
    <p class="intro">Si vous souhaitez des informations quant à ma disponibilité et le tarif d'une prestation, envoyer moi un <a href="mailto:jordan.nagadzina.sanchez@gmail.com">mail</a> ou contactez moi sur <a href="www.linkedin.com/in/jordannagadzina-sanchez"> LinkedIn </a>. Merci. </p>
    <p> Sinon, remplissez simplement le formulaire suivant.</p>
  </div>
  <form action="http://formspree.io/your@mail.com" method="POST">
    <label for="name">Name</label>
    <input type="text" id="name" name="name" class="full-width"><br>
    <label for="email">Email Address</label>
    <input type="email" id="email" name="_replyto" class="full-width"><br>
    <label for="message">Message</label>
    <textarea name="message" id="message" cols="30" rows="10" class="full-width"></textarea><br>
    <input type="submit" value="Send" class="button">
  </form>
</div>
