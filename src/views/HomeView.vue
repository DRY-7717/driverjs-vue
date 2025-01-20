<template>
  <div class="w-[500px] mx-auto mt-24">
    <div id="highlight-step-1" class="flex justify-between items-center p-3 test">
      <div class="box-input w-[200px]" id="input-1">
        <input class="w-full py-1 px-4 outline-none border-none rounded-md shadow-lg ring-1 ring-blue-300">
      </div>
      <button id="button-1" class="block px-6 py-1 text-white rounded-md bg-blue-500 shadow-lg">
        Submit
      </button>
    </div>
    <div id="highlight-step-2" class="flex justify-between items-center p-3 test">
      <div class="box-input w-[300px]" id="input-2">
        <input class="w-full py-1 px-4 outline-none border-none rounded-md shadow-lg ring-1 ring-blue-300">
      </div>
      <button id="button-2" class="block px-6 py-1 text-white rounded-md bg-blue-500 shadow-lg">
        Submit
      </button>
    </div>
  </div>
</template>

<script setup>
import { driver } from "driver.js";
import "driver.js/dist/driver.css";
import { onMounted } from "vue";
import Input from "@/components/Input.vue";
import Button from "@/components/Button.vue";


const driverObj = driver({
  allowClose: true,
  showProgress: true,
  showButtons: ['close'],
  popoverClass: 'customClass',
  stagePadding: 10,
  stageRadius: 10,
  progressText: '{{current}} dari {{total}}',
  steps: [
    {
      element: "#virtual-wrapper", // Soroti elemen virtual
      popover: {
        title: "Gabungan Input",
        description: "Dua elemen Input disorot bersama.",
        side: "bottom",
        align: "center",
      },
    },
    {
      element: "#virtual-wrapper-button", // Soroti elemen virtual
      popover: {
        title: "Gabungan Input",
        description: "Dua elemen Input disorot bersama.",
        side: "bottom",
        align: "center",
      },
    },
  ],
  onPopoverRender: (popover, { config, state }) => {

    // Create previous button
    const previousButtonCustom = document.createElement("button");
    previousButtonCustom.classList.add('btn-previous-custom');

    // Add disabled state for first step
    if (state.activeIndex === 0) {
      previousButtonCustom.classList.add('btn-previous-custom-disabled');
      previousButtonCustom.disabled = true;
    }
    popover.footerButtons.appendChild(previousButtonCustom);
    previousButtonCustom.addEventListener("click", () => {
      driverObj.movePrevious();
    });



    // Create next button
    const nextButtonCustom = document.createElement("button");
    nextButtonCustom.innerText = "Selanjutnya";
    nextButtonCustom.classList.add('btn-next-custom');
    popover.footerButtons.appendChild(nextButtonCustom);

    nextButtonCustom.addEventListener("click", () => {
      driverObj.moveNext();
    });

    if (driverObj.isLastStep()) {
      nextButtonCustom.innerText = "Selesai";
      nextButtonCustom.classList.add('btn-next-custom');
      popover.footerButtons.appendChild(nextButtonCustom);

      nextButtonCustom.addEventListener("click", () => {
        driverObj.destroy();
      });
    }

    // Create close button
    const closeButtonCustom = document.createElement("button");
    closeButtonCustom.classList.add('btn-close-custom');
    popover.closeButton.innerHTML = ''
    popover.closeButton.appendChild(closeButtonCustom);

    closeButtonCustom.addEventListener("click", () => {
      driverObj.destroy();
    });
  },
});
// ini untuk grouping 2 element yang berbeda lokasi
function createVirtualWrapper() {
  // Ambil elemen input
  const input1 = document.querySelector("#input-1");
  const input2 = document.querySelector("#input-2");

  // Buat elemen pembungkus
  let virtualWrapper = document.createElement("div");
  virtualWrapper.id = "virtual-wrapper";
  virtualWrapper.style.position = "absolute";

  // Hitung posisi dan ukuran elemen
  const rect1 = input1.getBoundingClientRect();
  const rect2 = input2.getBoundingClientRect();

  const top = Math.min(rect1.top, rect2.top);
  const left = Math.min(rect1.left, rect2.left);
  const width = Math.max(rect1.right, rect2.right) - left;
  const height = Math.max(rect1.bottom, rect2.bottom) - top;

  // Terapkan gaya ke elemen pembungkus
  virtualWrapper.style.top = `${top}px`;
  virtualWrapper.style.left = `${left}px`;
  virtualWrapper.style.width = `${width}px`;
  virtualWrapper.style.height = `${height}px`;
  virtualWrapper.style.pointerEvents = "none"; // Agar tidak mengganggu interaksi
  virtualWrapper.style.zIndex = "9999";

  // Tambahkan ke body
  document.body.appendChild(virtualWrapper);
}
function createVirtualWrapperButton() {
  // Ambil elemen input
  const input1 = document.querySelector("#button-1");
  const input2 = document.querySelector("#button-2");

  // Buat elemen pembungkus
  let virtualWrapperButton = document.createElement("div");
  virtualWrapperButton.id = "virtual-wrapper-button";
  virtualWrapperButton.style.position = "absolute";

  // Hitung posisi dan ukuran elemen
  const rect1 = input1.getBoundingClientRect();
  const rect2 = input2.getBoundingClientRect();

  const top = Math.min(rect1.top, rect2.top);
  const left = Math.min(rect1.left, rect2.left);
  const width = Math.max(rect1.right, rect2.right) - left;
  const height = Math.max(rect1.bottom, rect2.bottom) - top;

  // Terapkan gaya ke elemen pembungkus
  virtualWrapperButton.style.top = `${top}px`;
  virtualWrapperButton.style.left = `${left}px`;
  virtualWrapperButton.style.width = `${width}px`;
  virtualWrapperButton.style.height = `${height}px`;
  virtualWrapperButton.style.pointerEvents = "none"; // Agar tidak mengganggu interaksi
  virtualWrapperButton.style.zIndex = "9999";

  // Tambahkan ke body
  document.body.appendChild(virtualWrapperButton);
}


onMounted(() => {

  createVirtualWrapper();
  createVirtualWrapperButton();
  driverObj.drive();
});



</script>

<style>
.customClass {
  @apply bg-[#F8F8F8] rounded-2xl w-[295px] font-poppins
}

.customClass .driver-popover-title,
.customClass .driver-popover-description,
.customClass .driver-popover-progress-text {
  @apply text-[#005596] font-poppins
}

.customClass .driver-popover-title {
  @apply text-base font-[600] font-poppins
}

.customClass .driver-popover-description {
  @apply text-sm font-poppins
}

.customClass button.btn-next-custom,
.customClass button.btn-previous-custom {
  @apply font-poppins
}

.customClass button.btn-previous-custom-disabled,
.customClass button.btn-close-custom,
.customClass button.btn-previous-custom,
.customClass button.btn-next-custom {
  display: flex;
  text-align: center;
  text-shadow: none;
  font-size: 14px;
  padding: 6px 12px;
  align-items: center;
  justify-content: center;
}

.customClass button.btn-next-custom {
  background-color: #005596;
  color: white;
  border-radius: 6px;
  width: 109px;
  height: 32px;
}

.customClass button.btn-previous-custom {
  background-color: white;
  border-radius: 8px;
  width: 32px;
  height: 32px;
  background: url('../assets/arrow-active.svg');
  background-repeat: no-repeat;
  background-position: center;
  border: 1px solid #005596;
  outline: #005596;
}



.customClass button.btn-close-custom {
  background-color: white;
  border-radius: 6px;
  width: 32px;
  height: 32px;
  background: url('../assets/close.svg');
  background-repeat: no-repeat;
  background-position: center;
  border: none;
  margin-top: 10px;
  margin-left: -8px;
}

.customClass button.btn-previous-custom-disabled {
  border-radius: 6px;
  width: 32px;
  height: 32px;
  background: url('../assets/arrow.svg');
  background-repeat: no-repeat;
  background-position: center;
  border: none;
  outline: none;
}
</style>