<template>
  <div class="min-h-screen flex flex-col bg-white">
    <!-- Fixed header with back button -->
    <header class="fixed top-0 left-0 right-0 bg-white z-10">
      <div class="max-w-[95%] mx-auto px-2 py-4 sm:py-6">
        <NuxtLink 
          to="/" 
          class="inline-flex items-center hover:opacity-70 transition-opacity"
        >
          <img src="/assets/chevron-left.svg" alt="Back" class="h-6 w-6 sm:h-8 sm:w-8">
        </NuxtLink>
      </div>
    </header>
    <main class="flex-grow pt-32 pb-16">
      <div class="max-w-[95%] mx-auto px-2">
        <div class="max-w-3xl space-y-12">
          <!-- Imprint Section -->
          <section class="space-y-12">
            <h2 class="text-2xl font-semibold mb-12">Imprint</h2>
            <div>
              <p class="text-xl mb-2 font-medium">Information according to § 5 TMG</p>
            <p class="text-base">Nele Schaal<br />
            Rektor-Klaus-Straße 100<br />
            73525 Schwäbisch Gmünd<br />
            Deutschland</p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">Contact</p>
              <p class="text-base">E-Mail: nele.schaal@hfg-gmuend.de</p>
            </div>
          </section>

          <!-- Disclaimer Section -->
          <section class="space-y-12">
            <div>
              <p class="text-xl mb-2 font-medium">Liability for Content</p>
              <p class="text-base leading-relaxed">The contents of these pages were created with great care. However, I cannot guarantee the accuracy, completeness, or timeliness of the content. As a service provider, I am responsible for my own content on these pages in accordance with general laws (§ 7 Abs.1 TMG). However, according to §§ 8 to 10 TMG, I am not obligated to monitor transmitted or stored third-party information or to investigate circumstances that indicate illegal activity. Obligations to remove or block the use of information under general laws remain unaffected. However, liability in this regard is only possible from the time of knowledge of a specific infringement. Upon notification of such violations, I will remove this content immediately.</p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">Liability for Links</p>
              <p class="text-base leading-relaxed">My website contains links to external websites of third parties, on whose contents I have no influence. Therefore, I cannot assume any liability for these external contents. The respective provider or operator of the pages is always responsible for the contents of the linked pages. The linked pages were checked for possible legal violations at the time of linking. Illegal contents were not recognizable at the time of linking. However, a permanent content control of the linked pages is not reasonable without concrete evidence of a violation of the law. Upon notification of violations, I will remove such links immediately.</p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">Copyright</p>
              <p class="text-base leading-relaxed">The content and works created by the site operator on these pages are subject to German copyright law. Duplication, processing, distribution, or any form of commercialization of such material beyond the scope of the copyright law shall require the prior written consent of the respective author or creator. Downloads and copies of this site are only permitted for private, non-commercial use. Insofar as the content on this site was not created by the operator, the copyrights of third parties are respected. In particular, third-party content is marked as such. Should you nevertheless become aware of a copyright infringement, please inform me accordingly. Upon notification of violations, I will remove such content immediately.</p>
            </div>
          </section>

          <!-- Privacy Policy Section -->
          <section class="space-y-12 mt-32">
            <h2 class="text-2xl font-semibold mb-12">Privacy Policy</h2>
            <div>
              <p class="text-xl mb-2 font-medium">1. General Information</p>
              <p class="text-base leading-relaxed">I take the protection of your personal data very seriously. This website does not collect or process personal data except as described below.</p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">2. Access Data and Hosting</p>
              <p class="text-base leading-relaxed">When you visit this website, certain information (such as your IP address) is automatically transmitted to the server hosting this site. This is necessary for technical reasons and to display the website. This data is not used to identify you as a person.</p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">3. Google Fonts</p>
              <p class="text-base leading-relaxed">This website uses web fonts provided by Google (Google Fonts) for a consistent and appealing presentation. When you access a page, your browser loads the required web fonts from Google servers. In doing so, your IP address and information about your browser may be transmitted to Google.<br />
            <span class="font-semibold">Provider:</span> Google Ireland Limited, Gordon House, Barrow Street, Dublin 4, Ireland<br />
            <span class="font-semibold">Privacy Policy:</span> <a href="https://policies.google.com/privacy" target="_blank" rel="noopener noreferrer" class="underline">https://policies.google.com/privacy</a></p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">4. No Analytics, No Contact Form</p>
              <p class="text-base leading-relaxed">This website does <span class="font-semibold">not</span> use analytics tools, tracking, or a contact form. No personal data is collected, stored, or processed beyond what is technically necessary to display the website and load fonts.</p>
            </div>
            <div>
              <p class="text-xl mb-2 font-medium">5. Your Rights</p>
              <p class="text-base leading-relaxed">You have the right to obtain information about your stored personal data, its origin and recipients, and the purpose of data processing at any time free of charge. You also have the right to request the correction, blocking, or deletion of this data. To exercise these rights, please contact me at the email address provided above.</p>
            </div>
          </section>
        </div>
      </div>
    </main>
    <TheFooter :imprintLabel="'Imprint/Privacy Policy'" />
  </div>
</template>

<script setup lang="ts">
import TheFooter from '~/components/TheFooter.vue';
</script>

<style scoped>
/* Imprint page specific styles */
</style> 