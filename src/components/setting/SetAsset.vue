<template>
  <div class="set-asset">
    <!-- 1.목표금액 설정 2.각 자산들 설정 -->
    <h3 class="font-jua">목표 금액 설정</h3>
    <b class="explanation">* 모든 금액은 숫자만 기입해 주세요</b>

    <form action="">
      <div action="" class="total-goal-cont">
        <strong>총 목표 금액</strong>
        <input
          type="text"
          placeholder="총 목표 금액을 입력해 주세요"
          v-model="saveAsset.total"
        />
      </div>

      <div action="" class="money-goal-cont">
        <strong>현금 목표 금액</strong>
        <input
          type="text"
          placeholder="현금 목표 금액을 입력해 주세요"
          v-model="saveAsset.cash"
        />
      </div>

      <div action="" class="credit-goal-cont">
        <strong>은행 별 자산 입력</strong>
        <button @click.prevent="clickAddBank()">+</button>
        <ul>
          <li v-for="(bankList, index) in banks" :key="index">
            <select name="bank" v-model="saveAsset.bank">
              <option value="">은행선택</option>
              <option value="경남은행">경남은행</option>
              <option value="광주은행">광주은행</option>
              <option value="국민은행">국민은행</option>
              <option value="기업은행">기업은행</option>
              <option value="농협">농협</option>
              <option value="대구은행">대구은행</option>
              <option value="부산은행">부산은행</option>
              <option value="산업은행">산업은행</option>
              <option value="저축은행">저축은행</option>
              <option value="새마을금고">새마을금고</option>
              <option value="수협중항회">수협중항회</option>
              <option value="신협">신협</option>
              <option value="신한은행">신한은행</option>
              <option value="우리은행">우리은행</option>
              <option value="우체국">우체국</option>
              <option value="전북은행">전북은행</option>
              <option value="제주은행">제주은행</option>
              <option value="카카오뱅크">카카오뱅크</option>
              <option value="케이뱅크">케이뱅크</option>
              <option value="하나은행">하나은행</option>
              <option value="한국씨티은행">한국씨티은행</option>
              <option value="SC제일은행">SC제일은행</option>
            </select>
            <input
              type="text"
              placeholder="해당 은행의 총 목표 금액을 입력해 주세요"
              v-model="saveAsset.asset"
            />
            <!-- <button class="edit">수정</button> -->
            <button class="remove" @click.prevent="clickRemoveBank(bankList)">
              ✕
            </button>
          </li>
        </ul>
      </div>
      <!-- {{ banks }} -->
      {{ saveAsset }}
      <button
        v-if="
          this.$store.state.total === '' &&
            this.$store.state.cash === '' &&
            this.$store.state.bankAsset.bank === '' &&
            this.$store.state.bankAsset.asset === 0
        "
        @click.prevent="clickSaveAsset()"
      >
        설정
      </button>
      <button
        v-if="
          this.$store.state.total !== '' ||
            this.$store.state.cash !== '' ||
            this.$store.state.bankAsset.bank !== '' ||
            this.$store.state.bankAsset.asset !== 0
        "
        @click.prevent="clickSaveAsset()"
      >
        수정
      </button>
    </form>
  </div>
</template>

<script>
import { saveTotal, saveCash, saveBankAsset } from '@/utils/cookies.js';

export default {
  data() {
    return {
      banks: [{ bank: '', asset: 0 }],
      saveAsset: {
        total: this.$store.state.total,
        cash: this.$store.state.cash,
        bank: '',
        asset: 0,
        // banks: [{ bank: '', asset: 0 }],
      },
    };
  },
  created() {
    // --- cookie에 bank+asset를 합쳐 저장 해 놓은 것을 분리해서 data에 넣어줌. ---
    // cookie에 저장된 bankAsset 불러와 변수 선언.
    let getbankasset = this.$store.state.bankAsset.bank;
    // bank(문자(한글+영어)만 추출)
    this.saveAsset.bank = getbankasset.replace(/[0-9]/g, '');
    // asset(숫자만 추출)
    this.saveAsset.asset = getbankasset.replace(/[^0-9]/g, '');
  },
  methods: {
    clickAddBank() {
      // this.banks.push({ bank: '', asset: 0 });
      // this.setAsset.push({ bank: '' });
      // this.setAsset.push({ asset: 0 });
      this.banks.push({ bank: '', asset: 0 });
    },
    clickRemoveBank(bankList) {
      this.banks.$remove(bankList);
    },
    clickSaveAsset() {
      // 총 목표 금액 저장
      saveTotal(this.saveAsset.total);
      // 현금 목표 금액 저장
      saveCash(this.saveAsset.cash);
      // 은행 별 자산 저장(은행명+자산금액 묶어서)
      let bankAsset = `${this.saveAsset.bank}${this.saveAsset.asset}`;
      saveBankAsset(bankAsset);
    },
  },
};
</script>

<style></style>
