<script setup>
import { onMounted } from "vue";
import Card from "./components/card.vue";

function createStickySections(containerElement) {
  const container = {
    el: containerElement,
    height: 0,
    top: 0,
    bottom: 0,
  };
  const sections = Array.from(container.el.querySelectorAll("section"));
  let viewportTop = 0;
  let activeIndex = 0;
  let scrollValue = 0;

  function onScroll() {
    handleSections();
  }

  function initContainer() {
    container.el.style.minHeight = `${sections.length + 1}00vh`;
    container.el.classList.add("[&_*]:!transition-none");
    setTimeout(() => {
      container.el.classList.remove("[&_*]:!transition-none");
    }, 1);
  }

  function handleSections() {
    viewportTop = window.scrollY;
    container.height = container.el.clientHeight;
    container.top = container.el.offsetTop;
    container.bottom = container.top + container.height;

    if (container.bottom <= viewportTop) {
      scrollValue = sections.length + 1;
    } else if (container.top >= viewportTop) {
      scrollValue = 0;
    } else {
      scrollValue = remapValue(
        viewportTop,
        container.top,
        container.bottom,
        0,
        sections.length + 1
      );
    }
    activeIndex =
      Math.floor(scrollValue) >= sections.length
        ? sections.length - 1
        : Math.floor(scrollValue);

    sections.forEach((section, i) => {
      if (i === activeIndex) {
        section.style = "z-index: 1";
        section.firstChild.firstChild.style.opacity = "1";
        section.firstChild.lastChild.classList.add("scale-[1]");
        section.firstChild.lastChild.classList.remove("scale-[.8]");
        section.firstChild.lastChild.style.opacity = "1";
      } else {
        section.style = "z-index: 0";
        section.firstChild.firstChild.style.opacity = "0";
        section.firstChild.lastChild.classList.add("scale-[.8]");
        section.firstChild.lastChild.classList.remove("scale-[1]");

        section.firstChild.lastChild.style.opacity = "0";
      }
    });
  }

  function remapValue(value, start1, end1, start2, end2) {
    const remapped =
      ((value - start1) * (end2 - start2)) / (end1 - start1) + start2;
    return remapped > 0 ? remapped : 0;
  }

  function init() {
    initContainer();
    handleSections();
    window.addEventListener("scroll", onScroll);
  }

  init();
}

onMounted(() => {
  const sectionsContainer = document.querySelectorAll("[data-sticky-sections]");
  console.log(sectionsContainer);
  sectionsContainer.forEach((section) => {
    createStickySections(section);
  });
});

const cardData = [
  {
    id: 1,
    title: "Support your users with popular topics",
    description:
      "Statistics show that people browsing your webpage who receive live assistance with a chat widget are more likely to make a purchase.",
    imageUrl:
      "https://cruip-tutorials.vercel.app/sticky-scrolling/illustration-01.png",
  },
  {
    id: 2,
    title: "Personalise the support experience",
    description:
      "Statistics show that people browsing your webpage who receive live assistance with a chat widget are more likely to make a purchase.",
    imageUrl:
      "https://cruip-tutorials.vercel.app/sticky-scrolling/illustration-02.png",
  },
  {
    id: 3,
    title: "Scale your sales using automation",
    description:
      "Statistics show that people browsing your webpage who receive live assistance with a chat widget are more likely to make a purchase.",
    imageUrl:
      "https://cruip-tutorials.vercel.app/sticky-scrolling/illustration-03.png",
  },
  {
    id: 4,
    title: "Make customer satisfaction easier",
    description:
      "Statistics show that people browsing your webpage who receive live assistance with a chat widget are more likely to make a purchase.",
    imageUrl:
      "https://cruip-tutorials.vercel.app/sticky-scrolling/illustration-04.png",
  },
];

// Init StickySections
</script>

<template>
  <main
    class="relative min-h-screen flex flex-col justify-center bg-slate-50 overflow-hidden supports-[overflow:clip]:overflow-clip"
  >
    <div class="w-full max-w-6xl mx-auto px-4 md:px-6 py-24 scale-[1]">
      <div
        class="h-screen flex items-center justify-center text-4xl font-bold text-slate-300 text-center"
      >
        Scroll down
      </div>
      <div class="max-w-full mx-auto lg:max-w-none" data-sticky-sections>
        <div class="sticky top-0 h-screen space-y-0">
          <Card
            v-for="card in cardData"
            :title="card.title"
            :image-url="card.imageUrl"
            :id="card.id"
            :description="card.description"
            :total-length="cardData.length"
          />
        </div>
      </div>

      <div
        class="h-screen flex items-center justify-center text-4xl font-bold text-slate-300 text-center"
      >
        Scroll up
      </div>
    </div>
  </main>
</template>
