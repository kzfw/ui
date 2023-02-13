<template>
	<div class="card">
		<div class="card-content">
			<span class="card-title">New Training Request</span>
			<div class="loading_container" v-if="!milestones">
				<Spinner />
			</div>
			<div class="request_wrapper row row_no_margin" v-else>
				<div class="col s12 l6 push-l6">
					<p>This request page is solely for indicating <b>your</b> availability for your assigned instructor. You can find your assinged instructor by navigating to our Discord server and in the document in the #training-requests channel<br/><br />
					Once your assigned instructor has picked up your session, you will receive an email confirmation. Should another instructor pick up your session, you should assume they are a guest trainer and this has been coordinated internally. <br /><br />
					Prior to your session, ensure you have completed your assigned lesson in the <a href="https://sites.google.com/view/zfwacademy/" style="color:blue;">Fort Worth Academy</a> and that you have reviewed your latest training note(s).<br /><br />
					As always, please reach out to our training staff should you have any questions!</p>
				</div>
				<div class="col s12 l6 pull-l6">
					<form class="row row_no_margin" @submit.prevent=submitRequest>
						<div class="input-field col s12">
							<input id="start_date" type="text" ref="start_date" required>
							<label for="start_date">Start Time (Zulu)<span class="red-text">*</span></label>
						</div>
						<div class="input-field col s12">
							<input id="end_date" type="text" ref="end_date" required>
							<label for="end_date">End Time (Zulu)<span class="red-text">*</span></label>
						</div>
						<div class="input-field col s12">
							<select v-model="request.milestone" class="materialize-select">
								<option value="" disabled selected>Select a milestone</option>
								<option v-for="milestone in filteredMilestones" :key="milestone._id" :value="milestone.code">{{milestone.code + ' - ' + milestone.name}}</option>

							</select>
							<label>Milestone <span class="red-text">*</span></label>
						</div>
						<div class="input-field col s12">
							<textarea id="remarks" class="materialize-textarea" data-length="500" v-model="request.remarks"></textarea>
							<label for="remarks" class="active">Remarks</label>
						</div>
						<div class="submit_request">
							<input type="submit" class="btn" value="Request" :disabled="makingRequest" />
						</div>
					</form>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import { zabApi } from '@/helpers/axios.js';
import { mapState } from 'vuex';
import flatpickr from 'flatpickr';
import 'flatpickr/dist/flatpickr.min.css';

export default {
	name: 'RequestTraining',
	title: 'Request Training',
	data() {
		return {
			request: {
				milestone: '',
				remarks: '',
				submitter: ''
			},
			milestones: null,
			makingRequest: false
		};
	},
	async mounted() {
		await this.getTrainingMilestones();
		const today = new Date(new Date().toUTCString());

		M.FormSelect.init(document.querySelectorAll('select'), {});
		M.CharacterCounter.init(document.querySelectorAll('textarea'), {});

		this.request.submitter = this.user.data._id;

		flatpickr(this.$refs.start_date, {
			enableTime: true,
			time_24hr: true,
			minDate: today,
			disableMobile: true,
			minuteIncrement: 15,
			dateFormat: 'Y-m-dTH:i:00.000\\Z',
			altFormat: 'Y-m-d H:i',
			altInput: true,
            opacity: 100,
		});

		flatpickr(this.$refs.end_date, {
			enableTime: true,
			time_24hr: true,
			minDate: today,
			disableMobile: true,
			minuteIncrement: 15,
			dateFormat: 'Y-m-dTH:i:00.000\\Z',
			altFormat: 'Y-m-d H:i',
			altInput: true,
		});
	},
	methods: {
		async submitRequest() {
			try {
				if(!this.request.milestone) {
					this.toastError('You must select a milestone');
				} else {
					this.makingRequest = true;
					const {data} = await zabApi.post('/training/request/new', {
						...this.request,
						startTime: `${this.$refs.start_date.value}`,
						endTime: `${this.$refs.end_date.value}`
					});
					if(data.ret_det.code === 200) {
						this.toastSuccess('Training session requested');
						this.$router.push('/dash/training');
						this.makingRequest = false;
					} else {
						this.toastError(data.ret_det.message);
						this.makingRequest = false;
					}
				}
			} catch(e) {
				console.log(e);
			}
		},
		async getTrainingMilestones() {
			const {data} = await zabApi.get(`/training/milestones`);
			this.milestones = data.data.milestones;
		}
	},
	computed: {
		filteredMilestones() {
			const certs = this.user.data.certCodes;
			const rating = this.user.data.rating;
			if(this.milestones) {
				const minorPrerequisites = ["obs", "gnd", "twr", "app"];
				const majorPrerequisites = ["obs", "gnd", "ordgnd", "ordtwr", "ordapp"];
				let milestonesShowed = this.milestones.filter((milestone) => {
					if(this.user.data.vis) return (milestone.certCode.substring(0, 3) === "vis" && milestone.rating <= rating) || milestone.code === "GT1";
					else {
						return (  // This is still slightly hard to understand.  It returns the milestones that haven't been completed yet for the rating, or the C90 equivalent (if no major cert has been attained yet) and next rating's milestones, or center milestones if all other certs have been attained.
							!certs.includes(milestone.certCode) &&
							(
								milestone.code === "GT1" ||
								(milestone.certCode.substring(0, 3) === "ord" && certs.includes(milestone.certCode.slice(-3)) && certs.includes(majorPrerequisites[milestone.rating - 1])) || 
								(milestone.certCode.substring(0, 3) !== "ord" && (certs.includes(minorPrerequisites[milestone.rating - 1]) || (milestone.rating === "1" && certs.length === 0)) && milestone.certCode !== "zau") ||
								(milestone.certCode === "zau" && certs.includes("ordapp"))
							) && 
							milestone.certCode.substring(0, 3) !== "vis"
						);
					}
				});
				return milestonesShowed;
			}
		},
		...mapState('user', [
			'user'
		])
	}

};
</script>

<style scoped lang="scss">
.request_wrapper {
	padding-top: 1em;

	.col {
		margin-bottom: 1em;
	}
}

.submit_request {
	margin-left: .75em;
}
</style>
