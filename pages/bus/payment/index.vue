<template>
  <div v-if="getBookingInfoDetails !== null"
    class="lg:flex justify-between gap-x-5 py-4 lg:py-8 px-4 lg:px-[100px] bg-[#F7F7F7]">
    <div class="w-full lg:w-1/2">
      <div v-if="getBookingInfoDetails !== null"
        class="mt-4 bg-white rounded-[10px] border border-[#EDEDED] overflow-hidden">
        <div class="flex justify-start items-center gap-x-4 px-5 py-[16px] border-b">
          <div class="bg-[#FEF2F0] rounded-full w-9 h-9 flex justify-center items-center">
            <img src="@/assets/images/icons/ticket.svg" alt="" class="w-[15px]" />
          </div>
          <div>
            <h2 class="text-corporate text-sm lg:text-base font-medium">
              {{ getBookingInfoDetails.invoice.fromCity }} -
              {{ getBookingInfoDetails.invoice.toCity }}
            </h2>
            <p class="text-blackLight text-xs lg:text-sm font-normal mt-1">
              {{ getBookingInfoDetails.invoice.company }},
              {{
                String(getBookingInfoDetails.invoice.coachType).toUpperCase()
              }}
              bus | {{ boardingDateTime }}
            </p>
          </div>
        </div>
        <div class="px-[14px] py-3">
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Boarding Point:
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              {{ getBookingInfoDetails.invoice.boardingPoint }}
            </p>
          </div>
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Departure Time:
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              {{ boardingTime }}
            </p>
          </div>
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Name:
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              {{ getBookingInfoDetails.passenger.name }}
            </p>
          </div>
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Mobile Number:
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              +88{{ getBookingInfoDetails.passenger.phone }}
            </p>
          </div>
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Email Address:
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              {{ getBookingInfoDetails.passenger.email }}
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="w-full lg:w-1/2">
      <div class="mt-4 bg-white rounded-[10px] border border-[#EDEDED] overflow-hidden">
        <div class="flex justify-start items-center gap-x-4 px-5 py-[16px] border-b">
          <p class="text-base sm:text-xl font-medium text-blackPrimary">
            Fare Details
          </p>
        </div>
        <div class="px-[14px] pt-3">
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Seat No
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              {{ getBookingInfoDetails.invoice.seatNo.join(", ") }}
            </p>
          </div>
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Ticket Price
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              BDT
              {{
                showPromoInput
                  ? getBookingInfoDetails.payable
                  : getBookingInfoDetails.payable +
                  getBookingInfoDetails.invoice.discount
              }}
            </p>
          </div>

          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Processing Fee
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              BDT {{ getBookingInfoDetails.serviceCharge }}
            </p>
          </div>
          <div class="flex justify-between py-2 border-b last:border-b-0 border-dashed">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              Gateway Fee
            </p>
            <p class="text-xs lg:text-base font-medium text-blackPrimary">
              BDT {{ getBookingInfoDetails.paymentGatewayCommission }}
            </p>
          </div>
          <div v-if="
            getBookingInfoDetails?.invoice?.offer?.totalAmount ||
            getBookingInfoDetails?.invoice?.promo?.amount
          " class="flex justify-between py-2">
            <p class="text-[11px] leading-4 lg:text-sm font-normal text-blackLight">
              {{
                getBookingInfoDetails?.invoice?.offer?.totalAmount
                  ? "Offer"
                  : "Promo"
              }}
            </p>
            <div class="border border-[#F04935] h-6 px-2 rounded-full flex items-center">
              <p class="text-xs font-medium text-[#F04935]">
                {{
                  `- BDT ${getBookingInfoDetails?.invoice?.offer?.totalAmount ||
                  getBookingInfoDetails?.invoice?.promo?.amount
                  }`
                }}
              </p>
            </div>
          </div>
        </div>
        <div class="flex justify-between bg-[#EFF7FD] rounded-b p-4">
          <p class="text-sm font-normal text-blackLight">
            Total ({{ getBookingInfoDetails.invoice.seatNo.length }} seat{{
              getBookingInfoDetails.invoice.seatNo.length > 1 ? "s" : ""
            }})
          </p>
          <div class="flex flex-col-reverse md:flex-row items-end md:items-start">
            <div v-if="getBookingInfoDetails.invoice.discount"
              class="flex flex-row md:mr-4 items-center bg-[#48A43F] pl-[4px] pr-2 py-[2px] justify-center rounded-full mt-[4px] md:mt-0">
              <img src="@/assets/images/icons/promoIcon.svg" alt="" class="mr-[4px]" />
              <p class="text-xs font-medium text-[#FFF]">
                You save BDT
                <span class="text-sm">{{
                  getBookingInfoDetails.invoice.discount
                }}</span>
              </p>
            </div>
            <p class="text-base font-medium text-blackPrimary">
              <span class="font-bold">
                BDT {{ getBookingInfoDetails.amount }}</span>
            </p>
          </div>
        </div>
      </div>

      <div class="mt-4 bg-white rounded-[10px] border border-[#EDEDED]"
        v-if="!getBookingInfoDetails?.invoice?.offer?.totalAmount">
        <div class="flex justify-between items-center gap-x-4 px-5 py-[16px] border-b">
          <p class="text-base sm:text-xl font-medium text-blackPrimary">
            Promo
          </p>
        </div>
        <div class="flex gap-x-5 w-full flex-col">
          <div class="flex justify-between w-full p-4">
            <input :disabled="selectedPromoObjectIndex" type="text" id="promo" v-model="promoCode"
              placeholder="Enter Promo Code"
              class="bg-[#f7f7f7] px-4 py-[13px] rounded focus:outline-0 text-xs placeholder:text-blackSecondary text-blackPrimary custom-width" />
            <button v-if="selectedPromoObjectIndex === null" @click="() => applyPromo()" :disabled="!promoCode"
              class="w-[140px] md:w-[165px] rounded-full flex flex-nowrap flex-row items-center justify-center whitespace-nowrap bg-[#EFF7FD] text-[#156CB7]">
              <img src="@/assets/images/icons/blueTickBus.svg" alt="" class="mr-2" />
              <p class="text-sm font-medium">Add promo</p>
            </button>
            <button v-else @click="removePromo"
              class="w-[140px] md:w-[165px] rounded-full flex flex-nowrap flex-row items-center justify-center whitespace-nowrap bg-[#FDF0F1] text-[#C71C2D]">
              <img src="@/assets/images/icons/removePromoIcon.svg" alt="" class="mr-2" />
              <p class="text-sm font-medium">Remove promo</p>
            </button>
          </div>
          <div class="px-4" v-if="availablePromos?.length">
            <div class="w-full h-[1px] bg-[#EDEDED]" />
          </div>
          <div v-if="availablePromos?.length" class="p-4 pr-0">
            <div class="pb-4 w-full flex flex-row justify-between items-center pr-4">
              <p class="text-base sm:text-xl font-medium text-blackPrimary">
                Available promo
              </p>
              <div class="flex flex-row items-center">
                <div class="w-8 h-8 bg-[#F7F7F7] flex items-center justify-center mr-3 rounded-full"
                  @click="promoScrollLeft">
                  <img v-if="isLeftScrollDisabled" src="@/assets/images/icons/promoLeftArrow.svg" alt=""
                    class="h-4 w-4" />
                  <img v-if="!isLeftScrollDisabled" src="@/assets/images/icons/promoLeftActiveArrow.svg" alt=""
                    class="h-4 w-4" />
                </div>
                <div class="w-8 h-8 bg-[#F7F7F7] flex items-center justify-center rounded-full"
                  @click="promoScrollRight">
                  <img v-if="isRightScrollDisabled" src="@/assets/images/icons/promoRightArrow.svg" alt=""
                    class="h-4 w-4" />
                  <img v-if="!isRightScrollDisabled" src="@/assets/images/icons/promoRightActiveArrow.svg" alt=""
                    class="h-4 w-4" />
                </div>
              </div>
            </div>
            <div class="w-full flex overflow-x-scroll promo-container" ref="promoSlider">
              <div class="gap-x-4 flex flex-row">
                <PromoBox v-for="(promo, index) in availablePromos" :key="promo.code" :promo="promo"
                  :isLastItem="index === availablePromos.length - 1" :activePromo="activePromo"
                  :handlePromoBox="() => handlePromoBox(promo, index)" />
              </div>
            </div>
          </div>
        </div>
      </div>


      <div v-if="getBookingInfoDetails.amount >= 5000" class="mt-4 py-4 px-5 bg-white rounded-[10px] border border-[#EDEDED]">
        <div class="flex items-center justify-between gap-x-4">
          <div class="flex items-center md:items-start gap-x-2 md:gap-x-4">
            <img src="@/assets/images/icons/emi.png" alt="emi" class="shrink-0">
            <div>
              <h4 class="text-[16px] md:text-xl text-blackPrimary font-medium">EMI Available</h4>
              <p class="mt-1 text-sm text-[#676769]">Get EMI on bookings over 5000 BDT using credit card,<br class="hidden md:block"> select your plan in payment gateway.</p>
            </div>
          </div>
          <div class="shrink-0">
            <button @click="handleEmiModal" class="text-sm text-[#1E88E5] font-medium underline capitalize">
              See details
            </button>
            <EmiDetailsModal v-if="showEmiModal" @close-modal="showEmiModal = false" />
          </div>
        </div>

        <div class="mt-4 pt-4 border-t border-[#EDEDED] flex items-center gap-x-3">
          <div class="form-group">
            <input v-model="isPayWithEMI" type="checkbox" id="emi-pay" class="size-5 accent-green-600">
            <label for="emi-pay" class="text-blackPrimary font-medium">Pay with EMI</label>
          </div>
        </div>
      </div>

      <div class="mt-4 bg-white rounded-[10px] border border-[#EDEDED]">
        <div class="flex justify-between items-center gap-x-4 px-5 py-[16px] border-b">
          <p class="text-base sm:text-xl font-medium text-blackPrimary">
            Payment Method
          </p>
          <div
            class="flex justify-center items-center w-[139px] bg-[#F7F7F7] rounded-full text-base font-medium text-blackPrimary">
            <img src="@/assets/images/icons/timer.svg" alt="" class="w-[13.33px]" />
            <p class="text-[#E0293B] text-xs font-medium py-[5px] px-4">
              <CountDown :time="paymentValidateTime" @timeUp="timeUp" /> m left
            </p>
          </div>
        </div>
        <div class="p-4 flex justify-between gap-x-3">
          <BkashOption :disabled="isPayWithEMI" plan-name="bkash" plan-discount="10%" v-model="gatewayType" />
          <!-- <NagadOption
            plan-name="nagad"
            plan-discount="10%"
            v-model="gatewayType"
          /> -->
          <GateWayOption plan-name="sslcommerz" plan-discount="" v-model="gatewayType" />
        </div>
      </div>
      <div v-if="!paymentAllowStatus || paymentValidateTime === 0" class="mt-2">
        <PaymentTimeoutAlert />
      </div>

      <div class="text-center mt-4 lg:mt-5 flex flex-row items-center">
        <img v-if="!agreePrivacyPolicy" src="@/assets/images/icons/unCheckCircle.svg" alt="" class="cursor-pointer"
          @click="() => handleCheckBox()" />
        <img v-if="agreePrivacyPolicy" src="@/assets/images/icons/checkBoxCircle.svg" alt="" class="cursor-pointer"
          @click="() => handleCheckBox()" />
        <p class="text-blackPrimary text-sm font-medium text-left ml-2">
          By proceeding you are agreeing with our

          <span>
            <nuxt-link to="/bus/policies#terms-and-conditions" target="_blank"
              class="w-full underline text-[#1E88E5] font-medium">Terms and Conditions</nuxt-link>
            and
            <nuxt-link to="/bus/policies#return-and-refund-policy" target="_blank"
              class="w-full underline text-[#1E88E5] font-medium">Cancellation Policy</nuxt-link>
          </span>
        </p>
      </div>

      <LoaderButton class="bg-corporate rounded-full w-full py-[13px] text-white text-sm font-medium mt-6" :class="getLoading ||
        ((!agreePrivacyPolicy || !paymentAllowStatus) &&
          'bg-red-300 hover:bg-red-200 cursor-not-allowed')
        " :loading="getLoading" :disabled="getLoading ||
          !paymentAllowStatus ||
          paymentValidateTime === 0 ||
          !agreePrivacyPolicy
          " @onClick="paymentHandler">
        Pay Now
      </LoaderButton>

      <div v-if="!(!paymentAllowStatus || paymentValidateTime === 0)"
        class="p-5 mt-8 border border-[#DBDBDB] rounded-lg lg:hidden flex justify-start items-start gap-x-5">
        <img src="@/assets/images/icons/phoneIconBlack.svg" alt="" class="mt-2" />
        <div>
          <h2 class="text-base font-medium">Having Trouble?</h2>
          <p class="mt-[6px] text-sm text-[#8D8D8F] font-normal">
            Reach out to us for any kind of assistance on +09642080808
          </p>
          <a href="https://jatri.co/contact-us" target="_blank">
            <button class="text-xs font-medium bg-[#EDEDED] w-[95px] py-1 rounded-full mt-[10px]">
              Contact us
            </button></a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import EmiDetailsModal from "@/components/Modal/EmiDetailsModal.vue";
import ERROR_CODE from "@/constant/errorCodeEnum";
import { dateTimeFormat } from "@/helpers/dateTimeFormat";
import moment from "moment";
import { mapActions, mapGetters } from "vuex";
import {fireGTMEventForPaymentMethodSelection, fireGTMEventForPaymentInit, fireGTMEventForBookingConfirmed} from "@/helpers/AnalyticsEventHandler";
import {handleScrollBehaviour} from "@/helpers/utils";

export default {
  components: { EmiDetailsModal },
  middleware(ctx) {
    // ctx.$gtm.push({ event: "ssr" });
  },
  validate({ query }) {
    const { tnxId } = query;
    return tnxId;
  },
  data() {
    return {
      showEmiModal: false,
      gatewayType: "",
      activePromo: null,
      paymentAllowStatus: true,
      paymentValidateTime: 0,
      promoCode: "",
      agreePrivacyPolicy: true,
      isDown: false,
      startX: 0,
      scrollLeft: 0,
      sliderScrollLeft: 0, // Add this
      isLeftScrollDisabled: true,
      isRightScrollDisabled: true,
      selectedPromoObjectIndex: null,
      isPayWithEMI: false,
    };
  },
  watch: {
    showEmiModal(newValue) {
      const body = document.getElementsByTagName("body")[0];
      const html = document.getElementsByTagName("html")[0];

      if (newValue) {
        body.style.overflow = "hidden";
        html.style.overflow = "hidden";
      } else {
        body.style.overflow = "";
        html.style.overflow = "";
      }
    },
    async gatewayType() {
      const payload = {
        paymentId: this.getBookingInfoDetails._id,
        gatewayType: this.gatewayType,
      };
      if (this.getBookingInfoDetails?.gatewayType !== this.gatewayType) {
        try {
          await this.updateGatewayAction(payload);
        } catch (err) {
          if (
            err.response.data.error === ERROR_CODE.SEVERAL_TRANSACTION_ATTEMPT
          ) {
            this.handleSeveralPaymentAttempt(
              this.getBookingInfoDetails.invoice
            );
          }
          this.gatewayType = this.getBookingInfoDetails.gatewayType;
        }
      }
    },
    selectedPromoObjectIndex() {
      if (this.selectedPromoObjectIndex >= 0) {
        this.makeSelectedPromoCenter(this.selectedPromoObjectIndex);
      }
    },
    isPayWithEMI(value){
      if(value){
        this.gatewayType = 'sslcommerz'
      }
    }
  },
  mounted() {


    if (this.availablePromos?.length && this?.$refs?.promoSlider) {
      const slider = this.$refs.promoSlider;

      // Calculate max scroll position
      const maxScrollLeft = slider.scrollWidth - slider.clientWidth;

      // Initially set the button states based on the scroll position
      this.isLeftScrollDisabled = true;
      this.isRightScrollDisabled = maxScrollLeft <= 0;

      slider.addEventListener("mousedown", (e) => {
        this.isDown = true;
        slider.classList.add("active");
        this.startX = e.pageX - slider.offsetLeft;
        this.scrollLeft = slider.scrollLeft;
      });

      slider.addEventListener("mouseleave", () => {
        this.isDown = false;
        slider.classList.remove("active");
      });

      slider.addEventListener("mouseup", () => {
        this.isDown = false;
        slider.classList.remove("active");
      });

      slider.addEventListener("mousemove", (e) => {
        if (!this.isDown) return;
        e.preventDefault();
        const x = e.pageX - slider.offsetLeft;
        const walk = (x - this.startX) * 3;
        slider.scrollLeft = this.scrollLeft - walk;
      });
    }
  },
  async asyncData({ query, store }) {
    await store.dispatch("busStore/getBookingInfoByTnxId", {
      transactionId: query.tnxId,
    });
  },

  created() {
    if (this.getBookingInfoDetails) {
      this.paymentValidateTime = this.calculateSecondsLeft(
        this.getBookingInfoDetails?.pendingValidity?.split("T")[0] +
        " " +
        this.getBookingInfoDetails?.pendingValidity
          ?.split("T")[1]
          .split(".")[0]
      );

      const invoicePromoCode = this.getBookingInfoDetails?.invoice?.promo?.code;

      if (invoicePromoCode) {
        this.promoCode = invoicePromoCode;
        const getPromoObjectIndex =
          this.getBookingInfoDetails.availablePromos.findIndex(
            (promo) => promo.code === invoicePromoCode
          );

        if (getPromoObjectIndex >= 0) {
          this.activePromo =
            this.getBookingInfoDetails.availablePromos[getPromoObjectIndex];
          this.selectedPromoObjectIndex = getPromoObjectIndex;
        }
      }
      this.gatewayType = this.getBookingInfoDetails?.gatewayType || "";
    } else {
      this.paymentValidateTime = 0;
    }
  },
  computed: {
    ...mapGetters("busStore", ["getBookingInfoDetails", "getLoading"]),
    boardingDateTime() {
      return dateTimeFormat(
        this.getBookingInfoDetails.invoice.boardingDateTime,
        6,
        "ddd, DD MMM YYYY"
      );
    },
    availablePromos() {
      return this.getBookingInfoDetails.availablePromos.filter(
        (promo) => promo.title && promo.description && promo.code
      );
    },
    boardingTime() {
      return new Date(
        `${this.getBookingInfoDetails.invoice.boardingDateTime}`
      ).toLocaleString("en-Us", { timeStyle: "short" });
    },
    showPromoInput() {
      return this.getBookingInfoDetails?.invoice?.promo;
    },

  },
  methods: {
    ...mapActions("busStore", [
      "ticketConfirmAction",
      "applyPromoCodeAction",
      "removePromoCodeAction",
      "getSurpriseDealAction",
      "updateGatewayAction",
    ]),
    handleEmiModal() {
      // handleScrollBehaviour(this.showEmiModal);
      this.showEmiModal = !this.showEmiModal;
    },
    calculateSecondsLeft(timeToCompare) {
      const currentTime = moment();
      const targetTime = moment(timeToCompare);
      const diffInSeconds = targetTime?.diff(currentTime, "seconds");
      return diffInSeconds || 0;
    },
    handleSliderScroll() {
      this.updateSliderState();
    },

    promoScrollLeft() {
      this.scrollSlider(-276);
    },

    promoScrollRight() {
      this.scrollSlider(276);
    },
    updateSliderState() {
      const slider = this.$refs?.promoSlider;
      if (slider) {
        this.sliderScrollLeft = slider.scrollLeft;

        const maxScrollLeft = slider.scrollWidth - slider.clientWidth;
        this.isLeftScrollDisabled = this.sliderScrollLeft === 0;
        this.isRightScrollDisabled = this.sliderScrollLeft === maxScrollLeft;
      }
    },
    scrollSlider(amount) {
      const slider = this.$refs?.promoSlider;
      if (slider) {
        slider.scrollLeft += amount;
        this.updateSliderState();
      }
    },
    makeSelectedPromoCenter(index) {
      const promoSlider = this.$refs?.promoSlider;
      if (promoSlider) {
        const myStartPosition = index * 276;
        const scrollAmount = myStartPosition - promoSlider.scrollLeft;
        const centeringOffset = (promoSlider.clientWidth - 276) / 2;
        this.scrollSlider(scrollAmount - centeringOffset);
      }
    },

    handlePromoBox(promo, index) {
      this.selectedPromoObjectIndex = index;
      this.applyPromo(promo);
    },
    handleCheckBox() {
      this.agreePrivacyPolicy = !this.agreePrivacyPolicy;
    },
    handleDealButton() {
      this.$nextTick(async () => {
        this.$nuxt.$loading?.start();
        const payload = {
          companyId: this.getBookingInfoDetails.invoice?.companyId,
          tripDateTime:
            this.getBookingInfoDetails.invoice?.tripDateTime ||
            this.getBookingInfoDetails.invoice?.boardingDateTime,
          coachType: this.getBookingInfoDetails.invoice?.coachType,
          paymentId: this.getBookingInfoDetails._id,
        };
        this.getSurpriseDealAction(payload);

        this.$nuxt.$loading?.finish();
      });
    },
    async paymentHandler() {
      const payload = {
        paymentId: this.getBookingInfoDetails._id,
        gatewayType: this.gatewayType,
      };
      fireGTMEventForPaymentMethodSelection(this, this.gatewayType, this.selectedPromoObjectIndex?"YES":"NO", this.selectedPromoObjectIndex ? this.promoCode: "", (this.getBookingInfoDetails.invoice.discount ?? 0) + this.getBookingInfoDetails.amount, (this.getBookingInfoDetails.invoice.discount ?? 0), this.getBookingInfoDetails.amount)
      fireGTMEventForPaymentInit(this)
      this.$nextTick(async () => {
        try {
          this.$nuxt.$loading?.start();
          const { data } = await this.ticketConfirmAction(payload);
          if (data?.gatewayUrl) {
            const departureDateTime = moment(this.getBookingInfoDetails.invoice.departureDate + " " + this.getBookingInfoDetails.invoice.departureTime)
            // fireGTMEventForBookingConfirmed(this, this.getBookingInfoDetails.invoice.fromCity, this.getBookingInfoDetails.invoice.toCity, this.getBookingInfoDetails.invoice.boardingDateTime, departureDateTime, this.getBookingInfoDetails.invoice.boardingPoint, this.getBookingInfoDetails.invoice.droppingPoint, this.getBookingInfoDetails.invoice.seatNo.join(), this.getBookingInfoDetails.invoice.seatNo.length, (this.getBookingInfoDetails.invoice.discount ?? 0) + this.getBookingInfoDetails.amount, (this.getBookingInfoDetails.invoice.discount ?? 0), 0, this.getBookingInfoDetails.amount)
            window.location.href = data.gatewayUrl;
            this.$nuxt.$loading?.finish();
          }
        } catch (err) {
          if (
            err.response.data.error === ERROR_CODE.SEVERAL_TRANSACTION_ATTEMPT
          ) {
            this.handleSeveralPaymentAttempt(
              this.getBookingInfoDetails.invoice
            );
          }
          this.$toast.error(err.response.data.message, {
            position: "bottom-right",
            duration: 50000,
            containerClass: "padding: 100px",
          });
          this.$nuxt.$loading?.finish();
        }
      });
    },

    timeUp() {
      this.paymentAllowStatus = false;
    },
    applyPromo(promo) {

      this.$nextTick(async () => {
        const payload = {
          promoCode: promo?.code || this.promoCode,
          companyId: this.getBookingInfoDetails.invoice?.companyId,
          tripDateTime:
            this.getBookingInfoDetails.invoice?.tripDateTime ||
            this.getBookingInfoDetails.invoice?.boardingDateTime,
          coachType: this.getBookingInfoDetails.invoice?.coachType,
          paymentId: this.getBookingInfoDetails._id,
          tnxId: this.$route.query.tnxId,
        };

        try {
          // this.$nuxt.$loading?.start();
          const response = await this.applyPromoCodeAction(payload);

          if (response?.amount) {
            this.promoCode = promo?.code || this.promoCode;
            this.activePromo = promo || this.getPromoObject(this.promoCode);
          }

          this.$nuxt.$loading?.finish();
        } catch (error) {
          if(error.response.data.statusCode === 500){

          }

          if (
            error.response.data.error === ERROR_CODE.SEVERAL_TRANSACTION_ATTEMPT
          ) {
            this.handleSeveralPaymentAttempt(
              this.getBookingInfoDetails.invoice
            );
          }
          this.$nuxt.$loading?.finish();
        }
      });
    },
    getPromoObject(promoCode) {
      let active = this.getBookingInfoDetails?.availablePromos?.find(
        (promo) => promo.code.toLowerCase() === promoCode.toLowerCase()
      );
      if (active) {
        this.selectedPromoObjectIndex = active;
      }
      return active;
    },
    async removePromo() {
      this.$nextTick(async () => {
        this.$nuxt.$loading?.start();
        const payload = {
          paymentId: this.getBookingInfoDetails._id,
        };

        try {
          const success = await this.removePromoCodeAction(payload);

          if (success) {
            this.promoCode = "";
            this.activePromo = null;
            this.selectedPromoObjectIndex = null;
          } else {
            this.$toast.error("Failed to remove promo code", {
              position: "bottom-right",
              duration: 5000,
            });
          }
        } catch (error) {
          this.$toast.error(`An error occurred: ${error}`, {
            position: "bottom-right",
            duration: 5000,
          });
        } finally {
          this.$nuxt.$loading?.finish();
        }
      });
    },

    handleSeveralPaymentAttempt(invoice) {
      const query = {
        from: this.$route.query.from,
        to: this.$route.query.to,
        date: new Date(
          dateTimeFormat(invoice.boardingDateTime, 6, "DD MMM YYYY")
        ).getTime(),
        type: "all",
      };

      this.$router.push({ path: "/bus/trip", query }); // We can use replace but it will create inconsistant behavior for new tab
    },
  },
};
</script>

<style>
.promo-container {
  -ms-overflow-style: none;
  /* Internet Explorer 10+ */
  scrollbar-width: none;
  /* Firefox */
}

.promo-container::-webkit-scrollbar {
  display: none;
  /* Safari and Chrome */
}

.active {
  cursor: grab;
}

.custom-width {
  width: calc(100% - 156px);
}

@media only screen and (min-width: 768px) {
  .custom-width {
    width: calc(100% - 181px);
  }
}

.form-group input[type=checkbox] {
  position: relative;
  border: 2px solid #000;
  border-radius: 6px;
  background: none;
  cursor: pointer;
  line-height: 0;
  margin: -1px 8px 0 0;
  outline: 0;
  padding: 0 !important;
  vertical-align: text-top;
  height: 22px;
  width: 22px;
  -webkit-appearance: none;
  opacity: .5;
}

.form-group input[type=checkbox]:hover {
  opacity: 1;
}

.form-group input[type=checkbox]:checked {
  background-color: #68C160;
  border: 2px solid #68C160;
  opacity: 1;
}

.form-group input[type=checkbox]:before {
  content: '';
  position: absolute;
  right: 50%;
  top: 50%;
  width: 6px;
  height: 12px;
  border: solid #FFF;
  border-width: 0 2px 2px 0;
  margin: -1px -1px 0 -1px;
  transform: rotate(45deg) translate(-50%, -50%);
  z-index: 2;
}
</style>
