<template>
    <div class="address">
        <div class="button-group">
            <p>
                <button type="button" @click="onDisplayAddressEmbed">주소 찾기</button>
            </p>
            <p>
                <button type="button" class="button-primary" @click="onSend" :disabled="triggerSend">보내기</button>
            </p>
        </div>
        
        <div class="address-form" v-if="address !== null">
            <div>
                <span class="address-form-item">주소:      </span>
                <span v-text="`${zipcode} ${address}`"></span>
            </div>
            <div>
                <span class="address-form-item">추가 주소: </span>
                <input type="text" v-model="extendAddress" placeholder="부가적인 주소를 입력해주세요." style="width: 220px" :disabled="triggerSend"/>
            </div>
            <div v-if="triggerSend" style="margin: 16px 0">
                <span class="address-form-item">결과: </span>
                <span v-text="completeAddress"></span>
            </div>
        </div>
        
        <!-- 
            주소 폼을 표시할 위치입니다.
            ref를 이용해 위치를 지정하고 주소가 없을 경우에만
            v-show를 활용하여 폼을 표시해줍니다.
         -->
        <div ref="address-embed" class="address-embed" v-show="address === null"></div>
    </div>
</template>

<script>


export default {
    data() {
        return {
            address: null,
            zipcode: null,
            extendAddress: null,

            triggerSend: false,
        }
    },

    computed: {
        /**
         * 검색된 주소지와 추가로 입력한 주소 (아파트명, 단지 등)을 합쳐주는 계산식
         * @returns {string} 전체 주소
         */
        completeAddress() {
            if(!this.address && !this.zipcode)  return ''
            else if(!this.extendAddress)        return this.address
            return `${this.zipcode} ${this.address} ${this.extendAddress}`
        }
    },

    methods: {
        /**
         * 주소 검색을 클릭하면 호출합니다.
         */
        onDisplayAddressEmbed() {
            this.address = null
            this.zipcode = null
            this.extendAddress = null
            this.triggerSend = false

            new window.daum.Postcode({
                width: '100%',
                height: '100%',
                oncomplete: data => {
                    let road = data.roadAddress

                    let extraRoad = ''
                    
                    //  bname은 법정동 또는 법정리를 뜻하며
                    //  bname이 존재하고, bname의 내용 끝에 동, 로 또는 가 가 있을 경우입니다.
                    if(data.bname !== '' && /[동|로|가]$/g.test(data.bname))
                        extraRoad = data.bname

                    //  buldingName은 건물명, apartment는 공동주택 여부이다. (Y또는 N으로 반환)
                    //  건물명이 존재하고 공동주택일 경우 주소지에 건물명을 추가합니다.
                    if(data.buildingName !== '' && data.apartment === 'Y')
                        extraRoad += ((extraRoad !== '') ? ', ' + data.buildingName : data.buildingName)

                    if(extraRoad !== '')
                        extraRoad = ' (' + extraRoad + ')'
                    
                    if(road !== '')
                        road += extraRoad
                    
                    this.address = road
                    this.zipcode = data.zonecode    //  우편번호를 반환합니다.
                }
            }).embed(this.$refs['address-embed'])
        },

        onSend() {
            this.triggerSend = false

            if((!this.zipcode && !this.address) || !this.extendAddress)
                alert('주소를 입력해주세요.')
            else {
                this.triggerSend = true
                console.log('주소 결과: %c' + this.completeAddress, 'color:lime')
            }
        }
    },

    mounted() {
        this.$nextTick(() => {
            new window.daum.Postcode({
                width: '100%',
                height: '100%',
                oncomplete: data => {
                    let road = data.roadAddress

                    let extraRoad = ''
                    
                    //  bname은 법정동 또는 법정리를 뜻하며
                    //  bname이 존재하고, bname의 내용 끝에 동, 로 또는 가 가 있을 경우입니다.
                    if(data.bname !== '' && /[동|로|가]$/g.test(data.bname))
                        extraRoad = data.bname

                    //  buldingName은 건물명, apartment는 공동주택 여부이다. (Y또는 N으로 반환)
                    //  건물명이 존재하고 공동주택일 경우 주소지에 건물명을 추가합니다.
                    if(data.buildingName !== '' && data.apartment === 'Y')
                        extraRoad += ((extraRoad !== '') ? ', ' + data.buildingName : data.buildingName)

                    if(extraRoad !== '')
                        extraRoad = ' (' + extraRoad + ')'
                    
                    if(road !== '')
                        road += extraRoad
                    
                    this.address = road
                    this.zipcode = data.zonecode    //  우편번호를 반환합니다.
                }
            }).embed(this.$refs['address-embed'])
        })
    }
}
</script>

<style scoped>
.address {min-height: 550px;}
.button-group {display: flex;}
.button-group p, .button-group div {margin-right: 8px;}
.address-embed {position: relative; margin: 0 auto; height: 500px;}
.address-form {margin: 16px 0 16px;}
.address-form-item {font-weight: 600; white-space: pre;}
</style>