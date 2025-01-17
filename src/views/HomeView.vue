<template>
  <div class="w-[500px] mx-auto mt-24">
    <div class="flex justify-around items-center p-3 test">
      <Input />
      <Button />
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
  stagePadding: 7,
  stageRadius: 10,
  progressText: '{{current}} dari {{total}}',
  steps: [
    {
      element: '#input',
      popover: {
        title: 'Animated Tour Example',
        description: 'Here is the code example showing animated tour. Let\'s walk you through it.',
        side: "bottom",
        align: 'center'
      }
    },
    {
      element: '#button',
      popover: {
        title: 'Import the Library',
        description: 'It works the same in vanilla JavaScript as well as frameworks.',
        side: "bottom",
        align: 'center'
      }
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
        localStorage.setItem('isopen', true)
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


onMounted(() => {
  const hasTourCompleted = localStorage.getItem('isopen') === 'true';
  if (!hasTourCompleted) {
    driverObj.drive();
  }
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