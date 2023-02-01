<template>
	<div class="q-pa-md">
		<div class="q-gutter-md" style="max-width: 300px">
			<q-select outlined v-model="mainInfo.peopleNum" :options="peopleNumOptions" label="인원수"/>
			<q-input outlined v-model="mainInfo.price" label="전체금액"/>

			<q-input outlined v-model="mainInfo.alcohol" label="주류합계금액"/>
			<q-input outlined v-model="mainInfo.juice" label="음료수합계금액"/>

			<q-btn color="primary" label="확인" @click="submit()"/>
			<q-btn color="red" label="리셋" @click="reset()"/>
		</div>

		<!-- <div class="q-mt-md" v-if="isSubmit">
			<span class="text-weight-bolder">총 금액 : {{total}}원</span>
		</div> -->

		<div v-show="isSubmit" class="q-mt-md q-gutter-md" style="max-width: 300px" v-for="(item,i) in peopleInfo" :key="i">		
			<q-input filled v-model="peopleInfo[i].name" :label="'인원'+(i+1)+' 이름'"/>
			<q-checkbox dense v-model="peopleInfo[i].alcohol" label="술" color="teal" />
			<q-checkbox dense v-model="peopleInfo[i].juice" label="음료수" color="orange" />	
		</div>

		<div class="q-mt-md" v-if="isSubmit">
			<q-btn color="primary" label="계산하기" @click="calculate()"/>
		</div>

		<div id="capture">

			<!-- <div class="q-mt-md" v-if="isSubmit">
				총 계산 금액 : {{total}}원
			</div> -->

			<q-markup-table v-if="isCalculate" class="q-mt-lg" style="max-width: 300px">
				<thead>
					<tr>
						<th class="text-left"><span class="text-weight-bolder">이름</span></th>
						<th class="text-right"><span class="text-weight-bolder">정산금액</span></th>
					</tr>
				</thead>
				<tbody v-for="(item,i) in peopleInfo" :key="i">
					<tr>
						<td class="text-left">{{item.name}} 
							<span v-if="item.alcohol" style="color:red;font-size:12px">술o</span> <span v-if="item.juice" style="color:red;font-size:12px">음료수o</span>
						</td>
						<td class="text-right">{{item.price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')}}원</td>
					</tr>
				</tbody>
			</q-markup-table>

			<q-markup-table v-if="isCalculate" class="q-mt-sm" style="max-width: 300px">
				<thead>
					<tr>
						<th class="text-left"><span class="text-weight-bolder"></span></th>
						<th class="text-right"><span class="text-weight-bolder">
							정산금액 합계 : 
							{{totalCalprice.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')}}원</span>
						</th>
					</tr>
				</thead>
			</q-markup-table>
		</div>

		<div class="q-mt-md" v-if="isCalculate">
			<q-btn color="primary" label="캡쳐하기" @click="partShot()"/>
		</div>
	</div>
</template>

<style>
</style>

<script>
import html2canvas from 'html2canvas';
export default {
	name: 'Main',
	data(){
		return{
			mainInfo:{
				peopleNum:"",
				price:"",
				food:"",
				alcohol:"",
				juice:"",
			},
			peopleInfo:[], //name,alcohol,juice,price
			peopleNumOptions:[2,3,4,5,6,7,8,9,10],
			isSubmit : false,
			isCalculate : false,

			totalCalprice:0,
			
		}
	},
	computed:{
		total(){
			let totalprice = (this.mainInfo.food*1) + (this.mainInfo.alcohol*1) + (this.mainInfo.juice*1);
			return totalprice.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',');
		},
	},
	methods:{
		submit(){
			if(this.isSubmit){return;}
			if(!this.mainInfo.peopleNum){alert("인원수를 입력해줘");return;}
			else if(!this.mainInfo.price){alert("음식값을 입력해줘");return;}

			for(let i=0; i<this.mainInfo.peopleNum; i++){
				this.peopleInfo.push({name:"",alcohol:false,juice:false,price:0})
			}
			this.isSubmit=true;
		},
		reset(){
			this.peopleInfo=[];
			this.isSubmit=false;
			this.isCalculate=false;
			this.mainInfo={};
			this.totalCalprice = 0;
		},
		calculate(){
			this.totalCalprice = 0;
			let alcoholCnt = 0;
			let juiceCnt = 0;

			/** 전체 인원 중 술마신 인원 / 음료수 마신 인원 계산 */
			for(let i=0; i<this.peopleInfo.length; i++){
				if(this.peopleInfo[i].alcohol){alcoholCnt++;} 
				if(this.peopleInfo[i].juice){juiceCnt++;} 
			}		

			/** 전체인원의 최종 정산 할 금액 계산 */
			for(let i=0; i<this.peopleInfo.length; i++){
				let foodPrice = this.mainInfo.price*1-(this.mainInfo.alcohol*1+this.mainInfo.juice*1)
				let alcoholPrice = 0;
				let jucePrice = 0;		

				if(this.peopleInfo[i].alcohol==true){
					alcoholPrice = this.mainInfo.alcohol*1 / alcoholCnt;
				}

				if(this.peopleInfo[i].juice==true){
					jucePrice = this.mainInfo.juice*1 / juiceCnt;
				}

				this.peopleInfo[i].price = Math.round((foodPrice/this.peopleInfo.length)+alcoholPrice+jucePrice);
				this.totalCalprice+=this.peopleInfo[i].price;
			}	
			this.isCalculate = true;
		},
		/** 영역 캡쳐,클립보드 복사 */
		partShot(){
			html2canvas(document.querySelector("#capture")).then(canvas => {
				canvas.toBlob(blob => navigator.clipboard.write([new window.ClipboardItem({ [blob.type]: blob })]))
			});
		},
	}
}
</script>
