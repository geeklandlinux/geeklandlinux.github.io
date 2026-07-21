---
title: "Contacto"
description: "Formulario de contacto de Geekland"
summary: "Ponte en contacto conmigo para cualquier consulta, sugerencia o sugerencia de artículo."
layout: "single"
---

Si tienes alguna duda, sugerencia o propuesta, puedes enviarme un mensaje a través del siguiente formulario y te responderé lo antes posible:

<form action="https://formsubmit.co/jccall80@gmail.com" method="POST" style="max-width: 500px; margin: 20px 0; display: flex; flex-direction: column; gap: 15px;">
  <!-- Redirección tras enviar el mensaje (Opcional, vuelve a la home) -->
  <input type="hidden" name="_next" value="https://geeklandlinux.github.io/">
  <!-- Desactivar captcha si no lo deseas, o dejarlo activado -->
  <input type="hidden" name="_captcha" value="true">

  <div>
    <label for="name" style="display: block; margin-bottom: 5px; font-weight: bold;">Nombre:</label>
    <input type="text" id="name" name="name" required style="width: 100%; padding: 8px; border-radius: 4px; border: 1px solid #ccc; background: inherit; color: inherit;">
  </div>

  <div>
    <label for="email" style="display: block; margin-bottom: 5px; font-weight: bold;">Correo electrónico:</label>
    <input type="email" id="email" name="email" required style="width: 100%; padding: 8px; border-radius: 4px; border: 1px solid #ccc; background: inherit; color: inherit;">
  </div>

  <div>
    <label for="message" style="display: block; margin-bottom: 5px; font-weight: bold;">Mensaje:</label>
    <textarea id="message" name="message" rows="5" required style="width: 100%; padding: 8px; border-radius: 4px; border: 1px solid #ccc; background: inherit; color: inherit;"></textarea>
  </div>

  <button type="submit" style="padding: 10px 20px; background-color: var(--primary); color: var(--theme); border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">Enviar mensaje</button>
</form>
