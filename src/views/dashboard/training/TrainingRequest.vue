<template>
	<div class="card">
		<div class="card-content">
			<span class="card-title">Initial Training Request</span>
			<div class="loading_container">
				<Spinner />
			</div>
			<div class="request_wrapper row row_no_margin">
				<div class="col s12 l6 push-l6">
					<p>This request page is for signing up for training. <br /><br />
					Once an instructor has been assigned, you will receive an email with your instructor's name.<br /><br />
                    After you have been assigned an instructor, you may schedule a session using the training request form.<br /><br />
					As always, please reach out to our training staff should you have any questions!</p>
				</div>
				<div class="col s12 l6 pull-l6">
					<form class="row row_no_margin">
						<div class="input-field col s12">
							<select class="materialize-select">
								<option value="" disabled selected>Select a milestone</option>

							</select>
							<label>Rating <span class="red-text">*</span></label>
						</div>
						<div class="input-field col s12">
							<textarea id="remarks" class="materialize-textarea" data-length="500"></textarea>
							<label for="remarks" class="active">Remarks</label>
						</div>
						<div class="submit_request">
							<input type="submit" class="btn" value="Request" />
						</div>
					</form>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'RequestTraining',
	title: 'Request Training',
	data() {
		return {
		};
	},
	async mounted() {
	},
	methods: {
	},
	computed: {
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
