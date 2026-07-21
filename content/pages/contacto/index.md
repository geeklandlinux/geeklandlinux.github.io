---
title: "Contacto"
description: "Formulario de contacto de Geekland"
summary: "Ponte en contacto conmigo para cualquier consulta o sugerencia."
layout: "single"
---

Si tienes alguna duda, sugerencia o propuesta, puedes enviarme un mensaje a través del siguiente formulario y te responderé lo antes posible:

<form action="https://formsubmit.co/jccall80@gmail.com" method="POST" style="max-width: 500px; margin-top: 20px; display: flex; flex-direction: column; gap: 15px;">
  <!-- Redirección tras enviar el mensaje -->
  <input type="hidden" name="_next" value="https://geeklandlinux.github.io/">
  <input type="hidden" name="_captcha" value="true">

  <div style="display: flex; flex-direction: column; gap: 5px;">
    <label for="name" style="font-weight: bold;">Nombre:</label>
    <input type="text" id="name" name="name" required style="width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #444; background-color: #2b2b2e; color: #ffffff; font-size: 1rem;">
  </div>

  <div style="display: flex; flex-direction: column; gap: 5px;">
    <label for="email" style="font-weight: bold;">Correo electrónico:</label>
    <input type="email" id="email" name="email" required style="width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #444; background-color: #2b2b2e; color: #ffffff; font-size: 1rem;">
  </div>

  <div style="display: flex; flex-direction: column; gap: 5px;">
    <label for="message" style="font-weight: bold;">Mensaje:</label>
    <textarea id="message" name="message" rows="5" required style="width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #444; background-color: #2b2b2e; color: #ffffff; font-size: 1rem; resize: vertical;"></textarea>
  </div>

  <button type="submit" style="padding: 12px 24px; background-color: #22c55e; color: #ffffff; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; font-size: 1rem; align-self: flex-start;">Enviar mensaje</button>
</form>
