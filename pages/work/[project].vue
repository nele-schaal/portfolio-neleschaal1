<template>
  <div class="min-h-screen bg-white flex flex-col">
    <!-- Fixed header with back button -->
    <header class="fixed top-0 left-0 right-0 bg-white z-10">
      <div class="max-w-[95%] mx-auto px-2 py-4 sm:py-6">
        <NuxtLink 
          to="/" 
          class="inline-flex items-center hover:opacity-70 transition-opacity"
          @click.prevent="navigateBack"
        >
          <img :src="chevronLeft" alt="Back" class="h-6 w-6 sm:h-8 sm:w-8">
        </NuxtLink>
      </div>
    </header>

    <!-- Main content with padding to account for fixed header -->
    <div class="flex-grow max-w-[95%] mx-auto px-2 py-4 sm:py-8 pt-16 sm:pt-24">
      <!-- Project Header -->
      <div class="mb-4 sm:mb-12">
        <h1 class="text-4xl sm:text-6xl font-bold mb-2">{{ project?.title }}</h1>
        <p v-if="project?.subtitle" class="text-2xl sm:text-3xl font-normal">{{ project?.subtitle }}</p>
      </div>

      <!-- Project Image -->
      <div v-if="project?.image" class="w-full rounded-lg overflow-hidden mb-8">
        <img :src="project?.image" :alt="project?.title" class="w-full h-auto" />
      </div>

      <!-- Project Content Grid -->
      <div class="grid grid-cols-1 md:grid-cols-12 gap-6 md:gap-8">
        <!-- Metadata Section -->
        <div class="md:col-span-6">
          <div class="flex flex-row justify-between gap-4 flex-wrap sm:flex-row sm:gap-0">
            <!-- Team -->
            <div class="flex-1 min-w-[100px]">
              <p class="text-sm sm:text-base font-medium text-black mb-2">(Team)</p>
              <div v-if="project?.team" class="space-y-1">
                <p v-for="member in project?.team" :key="member" class="text-sm sm:text-base">{{ member }}</p>
              </div>
            </div>

            <!-- Duration -->
            <div class="flex-1 min-w-[100px]">
              <p class="text-sm sm:text-base font-medium text-black mb-2">(Duration)</p>
              <p v-if="project?.time" class="text-sm sm:text-base">{{ project?.time }}</p>
            </div>

            <!-- Topic -->
            <div class="flex-1 min-w-[100px]">
              <p class="text-sm sm:text-base font-medium text-black mb-2">(Topic)</p>
              <div v-if="project?.topic" class="space-y-1">
                <p v-for="item in project?.topic" :key="item" class="text-sm sm:text-base">{{ item }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Description Column -->
        <div class="md:col-span-6 md:col-start-7 mt-8 md:mt-0">
          <p v-if="project?.description" class="text-base sm:text-lg md:text-xl leading-relaxed whitespace-pre-line">{{ project?.description }}</p>
        </div>
      </div>

      <!-- INCREASED GAP BELOW THIS LINE ONLY FOR FIRST SECTION -->
      <div class="mt-8 sm:mt-32"></div>

      <!-- Alex-specific content -->
      <template v-if="projectSlug === 'alex'">
        <!-- Emotions Grid -->
        <div v-if="project?.emotions" class="mt-0 mb-32 sm:mb-48 hidden sm:block">
          <!-- Top row: first 3 emotions -->
          <div class="flex flex-col sm:flex-row mb-8 gap-8 sm:gap-0">
            <div 
              v-for="(emotion, index) in project.emotions.slice(0, 3)" 
              :key="emotion.name"
              class="relative rounded-lg overflow-hidden w-[250px] h-[250px] transition-all duration-300 hover:scale-[1.05] cursor-pointer group"
              :class="index > 0 ? 'ml-8' : ''"
              @mouseenter="playVideo($event)"
              @mouseleave="stopVideo($event)"
            >
              <video 
                :src="emotion.video"
                class="w-full h-full object-cover emotion-video"
                muted
                playsinline
                loop
              ></video>
              <div class="absolute bottom-4 left-[42%] -translate-x-1/2 w-[250px] text-center opacity-0 group-hover:opacity-100 transition-opacity duration-200">
                <span class="text-base">{{ emotion.name }}</span>
              </div>
            </div>
          </div>

          <!-- Bottom row: remaining 4 emotions -->
          <div class="flex flex-col sm:flex-row gap-8 sm:gap-0">
            <!-- Empty space to align with top row -->
            <div class="w-[250px] opacity-0 hidden sm:block"></div>
            
            <!-- First visible video (disgust) aligned with second video in top row -->
            <div 
              class="relative rounded-lg overflow-hidden w-[250px] h-[250px] transition-all duration-300 hover:scale-[1.05] cursor-pointer ml-8 group"
              @mouseenter="playVideo($event)"
              @mouseleave="stopVideo($event)"
            >
              <video 
                :src="project.emotions[3].video"
                class="w-full h-full object-cover emotion-video"
                muted
                playsinline
                loop
              ></video>
              <div class="absolute bottom-4 left-[42%] -translate-x-1/2 w-[250px] text-center opacity-0 group-hover:opacity-100 transition-opacity duration-200">
                <span class="text-base">{{ project.emotions[3].name }}</span>
              </div>
            </div>
            
            <!-- Second video (fear) aligned with third video in top row -->
            <div 
              class="relative rounded-lg overflow-hidden w-[250px] h-[250px] transition-all duration-300 hover:scale-[1.05] cursor-pointer ml-8 group"
              @mouseenter="playVideo($event)"
              @mouseleave="stopVideo($event)"
            >
              <video 
                :src="project.emotions[4].video"
                class="w-full h-full object-cover emotion-video"
                muted
                playsinline
                loop
              ></video>
              <div class="absolute bottom-4 left-[42%] -translate-x-1/2 w-[250px] text-center opacity-0 group-hover:opacity-100 transition-opacity duration-200">
                <span class="text-base">{{ project.emotions[4].name }}</span>
              </div>
            </div>
            
            <!-- Third video (anger) -->
            <div 
              class="relative rounded-lg overflow-hidden w-[250px] h-[250px] transition-all duration-300 hover:scale-[1.05] cursor-pointer ml-8 group"
              @mouseenter="playVideo($event)"
              @mouseleave="stopVideo($event)"
            >
              <video 
                :src="project.emotions[5].video"
                class="w-full h-full object-cover emotion-video"
                muted
                playsinline
                loop
              ></video>
              <div class="absolute bottom-4 left-[42%] -translate-x-1/2 w-[250px] text-center opacity-0 group-hover:opacity-100 transition-opacity duration-200">
                <span class="text-base">{{ project.emotions[5].name }}</span>
              </div>
            </div>
            
            <!-- Fourth video (neutrality) -->
            <div 
              class="relative rounded-lg overflow-hidden w-[250px] h-[250px] transition-all duration-300 hover:scale-[1.05] cursor-pointer ml-8 group"
              @mouseenter="playVideo($event)"
              @mouseleave="stopVideo($event)"
            >
              <video 
                :src="project.emotions[6].video"
                class="w-full h-full object-cover emotion-video"
                muted
                playsinline
                loop
              ></video>
              <div class="absolute bottom-4 left-[42%] -translate-x-1/2 w-[250px] text-center opacity-0 group-hover:opacity-100 transition-opacity duration-200">
                <span class="text-base">{{ project.emotions[6].name }}</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Research Section -->
        <div class="mb-8 sm:mb-24">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Research</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <p class="text-lg sm:text-xl leading-relaxed">Through an expert interview with a psychologist who specializes in alexithymia, we found out that therapy for those affected is primarily about imparting emotional knowledge and gradually approaching one's own feelings.</p>
          </div>
        </div>

        <!-- Three Stages Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Three Stages</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <p class="text-lg sm:text-xl leading-relaxed">That is why alex introduces users to the conscious perception and allocation of their emotions in three successive stages. Each stage requires more initiative and promotes the ability to recognize and understand one's own emotional state.</p>
          </div>
        </div>

        <!-- Stage One Section -->
        <div class="mb-8 sm:mb-24">
          <h2 class="text-xl sm:text-2xl font-medium mb-4">(1)</h2>
          <div class="w-full">
            <!-- Image and Video Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
              <!-- Left: Image -->
              <div>
                <img 
                  :src="stufeeinsImage" 
                  alt="Stage One Interface" 
                  class="w-full h-auto rounded-lg"
                />
              </div>
              <!-- Right: Autoplay Video (desktop only) -->
              <div class="hidden sm:block">
                <video 
                  :src="videoeinsVideo"
                  class="w-full h-auto rounded-lg"
                  autoplay
                  muted
                  loop
                  playsinline
                ></video>
              </div>
            </div>
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">The user receives notifications to listen to themselves regularly. The smartwatch displays biophysiological changes in the body, physical sensations that could occur due to the measured emotion and the emotion measured by alex to make the person concerned aware that these processes are all interrelated and influence each other.</p>
            </div>
          </div>
        </div>

        <!-- Stage Two Section -->
        <div class="mb-8 sm:mb-24">
          <h2 class="text-xl sm:text-2xl font-medium mb-4">(2)</h2>
          <div class="w-full">
            <!-- Image and Video Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
              <!-- Left: Image -->
              <div>
                <img 
                  :src="bildzweiImage" 
                  alt="Stage Two Interface" 
                  class="w-full h-auto rounded-lg"
                />
              </div>
              <!-- Right: Autoplay Video (desktop only) -->
              <div class="hidden sm:block">
                <video 
                  :src="videozweiVideo"
                  class="w-full h-auto rounded-lg"
                  autoplay
                  muted
                  loop
                  playsinline
                ></video>
              </div>
            </div>
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">In stage 2, users also see the measured emotion and are asked to enter their physical sensations into the smartwatch themselves, which is intended to promote the conscious perception of physical signals.</p>
            </div>
          </div>
        </div>

        <!-- Stage Three Section -->
        <div class="mb-8 sm:mb-24">
          <h2 class="text-xl sm:text-2xl font-medium mb-4">(3)</h2>
          <div class="w-full">
            <!-- Image and Video Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
              <!-- Left: Image -->
              <div>
                <img 
                  :src="bilddreiImage" 
                  alt="Stage Three Interface" 
                  class="w-full h-auto rounded-lg"
                />
              </div>
              <!-- Right: Autoplay Video (desktop only) -->
              <div class="hidden sm:block">
                <video 
                  :src="videodreiVideo"
                  class="w-full h-auto rounded-lg"
                  autoplay
                  muted
                  loop
                  playsinline
                ></video>
              </div>
            </div>
            <!-- Text Below -->
            <div class="grid grid-cols-1 gap-6 max-w-3xl text-left">
              <p class="text-lg sm:text-xl leading-relaxed">In stage 3, in addition to the physical sensations, the emotion felt itself should also be recorded, which in the long term strengthens awareness and the link between emotions and physical reactions.</p>
              <p class="text-lg sm:text-xl leading-relaxed">The levels can be changed at any time - either according to your own feelings or with a recommendation from alex, which comes when the self-assessment remains stable over a longer period of time.</p>
            </div>
          </div>
        </div>

        <!-- Home Screen Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Home Screen</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">On the home screen of the smartphone, the user always sees the currently measured emotion, together with any physical sensations that may occur as a result, as well as the change in biophysiological values in the body, regardless of which stage they are in.</p>
          </div>
          <img 
            :src="startseiteImage" 
            alt="Alex Home Screen Interface" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- Insights Page Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Insights Page</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">In the app on the smartphone, there is an Insights page for stages 2 and 3 that provides insights into the user's self-assessment. Here there are statistics and overviews that provide information about the process or progress of the self-assessment. These insights can be taken into therapy and viewed together with a therapist so that they can also gain an insight into the real emotional world of the person concerned and see where there are still difficulties in understanding their own emotions and what is already working well.</p>
          </div>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <div>
              <video 
                :src="insights1Video"
                class="w-full h-auto rounded-lg"
                autoplay
                muted
                loop
                playsinline
              ></video>
            </div>
            <div>
              <video 
                :src="insights2Video"
                class="w-full h-auto rounded-lg"
                autoplay
                muted
                loop
                playsinline
              ></video>
            </div>
          </div>
        </div>
      </template>

      <!-- Pulse-specific content -->
      <template v-if="projectSlug === 'pulse'">
        <!-- INCREASED GAP BELOW THIS LINE ONLY FOR FIRST SECTION -->
        <div class="mt-8 sm:mt-32"></div>
        <!-- Research Section -->
        <div class="mt-8 sm:mb-24">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Research</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <p class="text-lg sm:text-xl leading-relaxed">{{ project?.research }}</p>
          </div>
        </div>

        <!-- Prototype Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Prototype</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <p class="text-lg sm:text-xl leading-relaxed">{{ project?.prototype }}</p>
          </div>

          <!-- Style Guide Images Grid -->
          <div class="mt-12 flex flex-col lg:flex-row gap-6 items-center lg:items-start">
            <!-- All images stacked and smaller on mobile, row and full size on desktop -->
            <div class="flex flex-col lg:flex-row gap-6 w-full">
              <div class="flex flex-col gap-6 w-full lg:w-[550px] flex-shrink-0">
                <!-- Logo -->
                <div class="w-full flex justify-center lg:justify-start">
                  <img 
                    :src="logoPulseImage" 
                    alt="Pulse Logo" 
                    class="w-full max-w-sm lg:max-w-full h-auto"
                  />
                </div>
                <!-- Color Palette -->
                <div class="w-full flex justify-center lg:justify-start">
                  <img 
                    :src="farbePulseImage" 
                    alt="Pulse Color Palette" 
                    class="w-full max-w-sm lg:max-w-full h-auto"
                  />
                </div>
                <!-- Typography -->
                <div class="w-full flex justify-center lg:justify-start">
                  <img 
                    :src="fontPulseImage" 
                    alt="Pulse Typography" 
                    class="w-full max-w-sm lg:max-w-full h-auto"
                  />
                </div>
              </div>
              <!-- Components image -->
              <div class="w-full lg:flex-1 min-w-0 flex justify-center lg:justify-start">
                <img 
                  :src="componentsPulseImage" 
                  alt="Pulse Components" 
                  class="w-full max-w-sm lg:max-w-full h-auto rounded-lg"
                />
              </div>
            </div>
          </div>
        </div>

        <!-- Available Appointments Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Available Appointments</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">Users are presented with a clear overview of currently available blood donation appointments in their area, including key details such as date, time, number of participants, location, distance, and which friends are attending.</p>
          </div>
          <video 
            :src="terminbuchungVideo"
            class="w-full h-auto rounded-lg"
            autoplay
            muted
            loop
            playsinline
          ></video>
        </div>

        <!-- Challenge Function Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Challenge Function</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">Pulse features a challenge function that fosters friendly competition and encourages users to donate blood. Teams can compete to see who donates the most, boosting motivation and strengthening the sense of community. Additionally, this feature offers companies an appealing way to showcase their social responsibility in a visible and engaging manner.</p>
          </div>
          <img 
            :src="challengeImage" 
            alt="Challenge Function Interface" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- Donation History Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Donation History</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">The donation history displays the user's upcoming donation appointment, if one has been scheduled, followed by a list of past appointments. By clicking on any appointment, users can view an overview of the personal data recorded at the time, along with their health metrics and blood test results.</p>
          </div>
          <img 
            :src="spendeverlaufImage" 
            alt="Donation History Interface" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- Profile Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Profile</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">The profile page gives users meaningful insights into their impact in 2025, such as how many times they've donated, how many people they've potentially helped, and the total amount of blood they've given. These personal stats are designed to create an emotional connection to the cause. Below that, users can access key features like donation location management, their friends list, and their digital donor card.</p>
          </div>
          <video 
            :src="profilFlowVideo"
            class="w-full h-auto rounded-lg"
            autoplay
            muted
            loop
            playsinline
          ></video>
        </div>
      </template>

      <!-- Functions Section -->
      <div v-if="project?.functions && projectSlug !== 'sense' && projectSlug !== 'pulse'" class="mb-8 sm:mb-24">
        <div class="flex flex-col md:flex-row md:gap-32 lg:gap-40 mb-12">
          <h2 class="text-xl sm:text-2xl font-normal md:w-[150px] lg:w-[200px] md:flex-shrink-0 mb-4 md:mb-0">Functions</h2>
          <p class="text-base sm:text-lg leading-relaxed max-w-2xl">{{ project?.functions }}</p>
        </div>
        
        <!-- Functions Video - Autoplaying -->
        <div v-if="project?.functionsVideo" class="w-full rounded-lg overflow-hidden video-container">
          <video 
            class="max-w-[500px] w-full h-auto rounded-lg" 
            autoplay 
            muted 
            loop 
            playsinline
          >
            <source :src="project?.functionsVideo" type="video/mp4">
            Your browser does not support the video tag.
          </video>
        </div>
      </div>

      <!-- Challenge Section -->
      <div v-if="project?.challenge && projectSlug !== 'pulse'" class="mb-8 sm:mb-24">
        <div class="flex flex-col md:flex-row gap-8 items-start">
          <!-- Challenge Image - Left side on desktop -->
          <div v-if="project?.challengeImage" class="md:w-1/2 rounded-lg overflow-hidden">
            <img :src="project?.challengeImage" alt="Challenge" class="w-full h-auto" />
          </div>
          
          <!-- Challenge Text - Right side on desktop, aligned to bottom -->
          <div class="md:w-1/2 md:self-end">
            <p class="text-base sm:text-lg leading-relaxed">{{ project?.challenge }}</p>
          </div>
        </div>
      </div>

      <!-- Donation History Section -->
      <div v-if="project?.donationHistory && projectSlug !== 'pulse'" class="mb-8 sm:mb-24">
        <div class="flex flex-col md:flex-row-reverse gap-8 items-start">
          <!-- Donation History Image - Right side on desktop -->
          <div v-if="project?.donationHistoryImage" class="md:w-1/2 rounded-lg overflow-hidden">
            <img :src="project?.donationHistoryImage" alt="Donation History" class="w-full h-auto" />
          </div>
          
          <!-- Donation History Text - Left side on desktop, aligned to bottom -->
          <div class="md:w-1/2 md:self-end">
            <p class="text-base sm:text-lg leading-relaxed">{{ project?.donationHistory }}</p>
          </div>
        </div>
      </div>

      <!-- Sense-specific content -->
      <template v-if="projectSlug === 'sense'">
        <!-- INCREASED GAP BELOW THIS LINE ONLY FOR FIRST SECTION -->
        <div class="mt-8 sm:mt-32"></div>
        <!-- Real-time data Section -->
        <div class="mt-8 sm:mb-8">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Real-time data</h2>
          <div class="w-full">
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">Sense works with real-time data, featuring an integrated sensor that passively measures air quality at regular intervals. It also offers the option for active measurement, which can be triggered manually by pressing the orange button on the device.</p>
            </div>
          </div>
        </div>

        <!-- Measurement Section -->
        <div class="mb-8 sm:mb-24">
          <div class="w-full">
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">Depending on the measurement results, the LED strip on the front changes. The higher the pollen concentration in the air, the more it rises. Once the personal exposure threshold, previously set in the app, has been reached, the LED indicator is fully lit, and the allergy sufferer receives vibration feedback as an early warning.</p>
            </div>
          </div>
        </div>

        <!-- Images Grid -->
        <div class="mb-8 sm:mb-24">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-48">
            <!-- Left Image -->
            <div>
              <img 
                :src="buttonSenseImage" 
                alt="Sense Button Detail" 
                class="w-full h-auto rounded-lg"
              />
            </div>
            <!-- Right Image -->
            <div>
              <img 
                :src="schlitzSenseImage" 
                alt="Sense Slot Detail" 
                class="w-full h-auto rounded-lg"
              />
            </div>
          </div>
        </div>

        <!-- Emergency function Section -->
        <div class="mb-8 sm:mb-24">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Emergency function</h2>
          <div class="w-full">
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">Sense features an emergency function that can be activated by sliding open the upper part of the device. If a high pollen exposure leads to an emergency, the wearer can press the SOS button to alert their emergency contacts stored in the app. The device also includes fall detection and automatically opens so that others can scan the QR code to access the emergency profile and provide assistance.</p>
            </div>
          </div>
        </div>

        <!-- Emergency function Image -->
        <div class="mb-8 sm:mb-24 mb-48">
          <img 
            :src="notfallfunktionImage" 
            alt="Emergency Function Illustration" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- App Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">App</h2>
          <div class="w-full">
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">The app allows the user to set their personal threshold levels for the pollens they are allergic to, as well as configure an emergency profile and store emergency contacts.</p>
            </div>
          </div>
        </div>

        <!-- App Image -->
        <div class="mb-8 sm:mb-24">
          <img 
            :src="belastungImage" 
            alt="App Interface" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- Usability Testing Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Usability Testing</h2>
          <div class="w-full">
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">To test our concept and identify any potential issues or ambiguities, we built a functional prototype using Arduino and evaluated it with five participants as part of a usability test.</p>
            </div>
          </div>
        </div>

        <!-- Community Section -->
        <div class="mb-8 sm:mb-24 mt-24 sm:mt-48">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Community</h2>
          <div class="w-full">
            <!-- Text Below -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
              <p class="text-lg sm:text-xl leading-relaxed">With Sense, allergy sufferers become part of a community that supports one another, because each device acts as a new measuring station, collecting data to improve pollen forecasts. By protecting yourself, you're also protecting others.</p>
            </div>
          </div>
        </div>

        <!-- Community Map Image -->
        <div class="mb-8 sm:mb-24">
          <img 
            :src="mapSenseImage" 
            alt="Sense Community Map" 
            class="w-full h-auto rounded-lg"
          />
        </div>
      </template>

      <!-- Sound Plotter content -->
      <template v-if="projectSlug === 'sound plotter'">
        <!-- INCREASED GAP BELOW THIS LINE ONLY FOR FIRST SECTION -->
        <div class="mt-8 sm:mt-32"></div>
        <!-- Components Section -->
        <div class="mt-8 sm:mb-24">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Components</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">To build our device, we used an Arduino Uno R3, a Base Shield, a stepper motor driver, two stepper motors, a potentiometer, a button and an LCD display. The housing is made of laser-cut and hand-sawn wooden components that provide structural support and hold all parts in place.</p>
          </div>
          <img 
            :src="componentsPlotterImage" 
            alt="Sound Plotter Components" 
            class="w-full h-auto rounded-lg mb-8 sm:mb-48" 
          />
        </div>

        <!-- Functionality Section -->
        <div class="mb-8 sm:mb-24">
          <h2 class="text-xl sm:text-3xl font-medium mb-4">Functionality</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">An MP3 file is loaded into Processing and played while an FFT (Fast Fourier Transform) analysis is used to break down the music into its frequency spectrum. From this, the most dominant frequency is calculated and sent to the Arduino. At the same time, a potentiometer provides input that the Arduino reads and sends back to Processing, allowing the music's frequency output to be adjusted dynamically in real time.</p>
          </div>
          <img 
            :src="processingPlotterImage" 
            alt="Sound Plotter Processing Visualization" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- Motor Control Section -->
        <div class="mb-8 sm:mb-24">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">On the Arduino side, the incoming frequency values are translated into precise movements of two stepper motors. One motor controls the vertical motion of a pen, raising and lowering it in response to the frequency data, while the second motor steadily moves the paper forward. Together, they produce a continuous line drawing that visually represents the sound.</p>
          </div>
          <img 
            :src="functionalPlotterImage" 
            alt="Sound Plotter Functional Diagram" 
            class="w-full h-auto rounded-lg"
          />
        </div>

        <!-- Interactive Control Section -->
        <div class="mb-8 sm:mb-24">
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
            <p class="text-lg sm:text-xl leading-relaxed">An LCD display shows the current song title as scrolling text, we chose Girls Just Want to Have Fun by Cyndi Lauper, and a button allows users to start or stop the motorized drawing process, giving control over playback and plotting.
            <br><br>
            The potentiometer doesn't just adjust the musical frequency but also acts as an interactive control element within the system. By turning the potentiometer, users can change the pitch of the song, which directly affects the frequency analysis and, in turn, the drawing.
            Since the music is played loudly, users can not only see but also hear the changes they make. This creates a synchronized, real-time interaction between sound, motion, and image.</p>
          </div>
          
          <!-- Images Grid -->
          <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
            <!-- Left Image -->
            <div>
              <img 
                :src="device1PlotterImage" 
                alt="Sound Plotter Device Front View" 
                class="w-full h-auto rounded-lg"
              />
            </div>
            <!-- Right Image -->
            <div>
              <img 
                :src="device2PlotterImage" 
                alt="Sound Plotter Device Side View" 
                class="w-full h-auto rounded-lg"
              />
            </div>
          </div>
        </div>
      </template>
    </div>
    
    <!-- More work section -->
    <section class="mt-32 mb-8">
      <div class="max-w-[95%] mx-auto px-2">
        <hr class="mb-14 w-full border-t border-gray-400" />
        <h2 class="text-2xl font-normal mb-6">More work</h2>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-x-8 gap-y-8">
          <NuxtLink
            v-for="other in Object.values(projectsData).filter(p => p.title.toLowerCase() !== projectSlug).slice(0, 2)"
            :key="other.title"
            :to="`/work/${other.title.toLowerCase()}`"
            class="block group"
          >
            <div class="aspect-[4/3] rounded-lg overflow-hidden bg-gray-200 w-full transition-transform duration-300 group-hover:scale-[1.02] relative">
              <img v-if="other.image" :src="other.image" :alt="other.title" class="w-full h-full object-cover transition-transform duration-300 group-hover:scale-105" />
              <div class="absolute bottom-4 left-4 text-black transition-opacity duration-300 opacity-0 group-hover:opacity-100 text-xl sm:text-2xl">
                {{ other.title }}
              </div>
            </div>
          </NuxtLink>
        </div>
      </div>
    </section>
    <TheFooter />
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import pulseImage from '~/assets/pulse/pulse.png';
import logoPulseImage from '~/assets/pulse/logo-pulse.png';
import farbePulseImage from '~/assets/pulse/farbe-pulse.png';
import fontPulseImage from '~/assets/pulse/font-pulse.png';
import componentsPulseImage from '~/assets/pulse/components-pulse.png';
import challengeImage from '~/assets/pulse/challenge.png';
import spendeverlaufImage from '~/assets/pulse/spendeverlauf.png';
import profilFlowVideo from '~/assets/pulse/Profil-Flow.mp4';
import terminbuchungVideo from '~/assets/pulse/Terminbuchung.mp4';
import alexImage from '~/assets/alex/alex.png';
import chevronLeft from '~/assets/chevron-left.svg';
import joyVideo from '~/assets/alex/joy.mp4';
import contentmentVideo from '~/assets/alex/contentment.mp4';
import sadnessVideo from '~/assets/alex/sadness.mp4';
import disgustVideo from '~/assets/alex/disgust.mp4';
import fearVideo from '~/assets/alex/fear.mp4';
import angerVideo from '~/assets/alex/anger.mp4';
import neutralityVideo from '~/assets/alex/neutrality.mp4';
import stufeeinsImage from '~/assets/alex/stufeeins.png';
import videoeinsVideo from '~/assets/alex/videoeins.mp4';
import bildzweiImage from '~/assets/alex/bildzwei.png';
import videozweiVideo from '~/assets/alex/videozwei.mp4';
import bilddreiImage from '~/assets/alex/bilddrei.png';
import videodreiVideo from '~/assets/alex/videodrei.mp4';
import startseiteImage from '~/assets/alex/Startseite.png';
import insights1Video from '~/assets/alex/Insights1.mp4';
import insights2Video from '~/assets/alex/Insights2.mp4';
import senseImage from '~/assets/sense/sense.jpg';
import buttonSenseImage from '~/assets/sense/button-sense.jpg';
import schlitzSenseImage from '~/assets/sense/schlitz-sense.jpg';
import notfallfunktionImage from '~/assets/sense/notfallfunktion.png';
import belastungImage from '~/assets/sense/belastung.png';
import usabilityImage from '~/assets/sense/usability.png';
import mapSenseImage from '~/assets/sense/map-sense.png';
import soundPlotterImage from '~/assets/soundplotter/soundplotter.png';
import device1PlotterImage from '~/assets/soundplotter/device1_plotter.jpg';
import device2PlotterImage from '~/assets/soundplotter/device2_plotter.jpg';
import processingPlotterImage from '~/assets/soundplotter/processing_plotter.png';
import functionalPlotterImage from '~/assets/soundplotter/functional_plotter.png';
import componentsPlotterImage from '~/assets/soundplotter/components_plotter.png';
import TheFooter from '~/components/TheFooter.vue';

const route = useRoute();
const projectSlug = route.params.project as string;
const router = useRouter();

interface Project {
  title: string;
  subtitle?: string;
  image?: string;
  description?: string;
  team?: string[];
  time?: string;
  topic?: string[];
  research?: string;
  prototype?: string;
  emotions?: {
    name: string;
    video: string;
  }[];
  measurement?: string;
  functions?: string;
  functionsVideo?: string;
  challenge?: string;
  challengeImage?: string;
  donationHistory?: string;
  donationHistoryImage?: string;
}

const projectsData: Record<string, Project> = {
  alex: {
    title: 'alex',
    subtitle: 'Emotion detection in emotionally blind people',
    image: alexImage,
    team: ['Leon Bork', 'Nele Schaal'],
    time: '5 months',
    topic: ['Research','Concept','Prototype','UI/UX'],
    emotions: [
      { name: 'joy', video: joyVideo },
      { name: 'contentment', video: contentmentVideo },
      { name: 'sadness', video: sadnessVideo },
      { name: 'disgust', video: disgustVideo },
      { name: 'fear', video: fearVideo },
      { name: 'anger', video: angerVideo },
      { name: 'neutrality', video: neutralityVideo }
    ],
    description: 'Around 10% of the world\'s population is affected by alexithymia, which means they are emotionally blind. This personality trait describes the inability or considerable difficulty in perceiving and identifying one\'s own emotions and putting them into words.\nThose affected do have feelings, but they cannot consciously grasp or verbalize them. Instead of recognizing sadness, anger or fear as emotions, they often only feel physical reactions, such as pressure in the chest or inner tension. They lack awareness of the connection between emotions and physical perceptions, which restricts them, especially when interacting with other people.\n\nThat\'s why we designed alex, a therapy-accompanying app for smartphones and smartwatches that helps emotionally blind people to recognize seven of their emotional states and to understand and learn about them themselves in the future.'
  },
  pulse: {
    title: 'Pulse',
    subtitle: 'A concept for a blood donation app to increase donation rates among young people in Germany',
    image: pulseImage,
    team: ['Leon Bork', 'Enes Cilingir', 'Nele Schaal'],
    time: '5 months',
    topic: ['Research', 'Wireframes', 'Prototype', 'UI/UX'],
    description: 'There is an annual shortfall of 2.5 million blood donations in Germany, a deficit that arises because only 3% of the population donate regularly. Young people in particular rarely or never donate blood, as the topic is often not part of their everyday lives. This is exactly where Pulse comes in: An app that uses targeted functions to appeal to young people in particular and motivate them to donate blood regularly.',
    research: 'To gain insights, understand user needs, and identify existing problems, we used various research methods to inform the further design process. We conducted online surveys, shadowed a person during a blood donation, explored discussions in the DRK forum and conducted interviews. Additionally, we developed User Personas, formulated How-might-we questions, and used the Value Proposition Canvas to define key user needs and align our solution accordingly.',
    prototype: 'After creating wireframes in Figma, we defined a style guide for colors, fonts, and components to ensure a consistent and efficient workflow as a team.'
  },
  sense: {
    title: 'Sense',
    subtitle: 'A device for real-time pollen measurement to provide early warning for allergy sufferers',
    image: senseImage,
    team: ['Leon Bork', 'Nele Schaal'],
    time: '5 months',
    topic: ['Prototyping', 'Arduino', 'Usability Testing', 'Cinema 4D'],
    description: 'In Germany, 12 million people suffer from a pollen allergy and have to rely on inaccurate, out-of-date measurement data due to a lack of pollen measuring stations. As a result, warnings about high levels of exposure usually come too late, namely when sufferers are already developing the first symptoms. At this point, however, it is often too late for effective preventative measures and those affected have to suffer from the symptoms of their allergy.\n\nThat is why we have developed a concept for a device that warns allergy sufferers early and precisely of excessive pollen exposure, helping them to avoid symptoms and improve their quality of life.',
    functions: 'Sense works with real-time data, featuring an integrated sensor that passively measures air quality at regular intervals. It also offers the option for active measurement, which can be triggered manually by pressing the orange button on the device.',
    measurement: 'Depending on the measurement results, the LED strip on the front changes. The higher the pollen concentration in the air, the more it rises. Once the personal exposure threshold, previously set in the app, has been reached, the LED indicator is fully lit, and the allergy sufferer receives vibration feedback as an early warning.'
  },
  "sound plotter": {
    title: 'Sound Plotter',
    subtitle: 'Rapid prototyping of a mechanical drawing device',
    image: soundPlotterImage,
    team: ['Helen Frank', 'Nele Schaal'],
    time: '5 days',
    topic: ['Rapid prototyping', 'Visualization', 'Arduino', 'Processing'],
    description: 'In the 5-day mechatronics workshop focused on rapid prototyping, we were tasked with developing a mechanical drawing device capable of creating lines and shapes on surfaces.\nAfter a short brainstorming phase, we decided to prototype a device that analyzes the frequencies of a song in real time and visualizes them on paper using a pen, controlled by stepper motors and serial communication between Processing and Arduino.'
  }
};

const project = ref<Project | null>(projectsData[projectSlug as keyof typeof projectsData] || null);

function playVideo(event: MouseEvent) {
  const video = (event.currentTarget as HTMLElement).querySelector('video');
  if (video) {
    video.currentTime = 0;
    video.play();
  }
}

function stopVideo(event: MouseEvent) {
  const video = (event.currentTarget as HTMLElement).querySelector('video');
  if (video) {
    video.pause();
    video.currentTime = 0;
  }
}

async function navigateBack(event: Event) {
  event.preventDefault();
  await router.push('/');
  // Wait for the next DOM update cycle
  await nextTick();
  // Add a small delay to ensure the section is mounted
  await new Promise(resolve => setTimeout(resolve, 100));
  const workSection = document.getElementById('work');
  if (workSection) {
    workSection.scrollIntoView({ behavior: 'smooth' });
  }
}
</script>

<style scoped>
/* Removing fixed aspect ratio constraint to prevent cropping */

/* Video container styling */
.video-container {
  position: relative;
  background-color: transparent;
  line-height: 0; /* Remove extra space below video */
}

/* Ensuring video fills container */
.video-container video {
  max-width: 500px; /* Limit video width */
  width: 100%;
  display: block;
  border-radius: 8px;
  /* box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05); */
}

@media (max-width: 768px) {
  .video-container video {
    max-width: 100%; /* Full width on mobile */
  }
}

/* Add specific styling for emotion videos */
.emotion-video {
  aspect-ratio: 1;
  width: 200px;
  height: 200px;
  transition: all 0.3s ease;
  /* box-shadow: none !important; */
}

.emotion-video:hover {
  transform: scale(1.02);
  /* box-shadow: 0 2px 8px rgba(0,0,0,0.05); */
}
</style> 