<template>
	<div>
		<h4 class="text-center mb-4">
			<strong>Estimated Sell Price:</strong> {{ price.gb }} GB {{ price.sb }} SB
		</h4>
		<form method="POST" id="liogene_form">
			<div class="row">
				<div class="col col-lg-6 col-md-6 col-sm-12 col-12">
					<!-- base color -->
					<label for="base_color" class="mt-2 mb-0">Base Color</label>
					<select name="base_color" class="form-control" v-model="form.base_color">
						<option selected disabled :value="null">-- Select a Gene --</option>
						<optgroup v-for="(_bases, group) in bases" :label="group">
							<option v-for="base in _bases" :value="base.id">{{ base.name }}</option>
						</optgroup>
					</select>
					<!-- eye color -->
					<label for="eye_color" class="mt-2 mb-0">Eye Color</label>
					<select name="eye_color" class="form-control" v-model="form.eye_color">
						<option selected disabled :value="null">-- Select a Gene --</option>
						<option v-for="eye in eyes" :value="eye.id">{{ eye.name }}</option>
					</select>
					<!-- mane shape -->
					<label for="mane_shape" class="mt-2 mb-0">Mane Shape</label>
					<select name="mane_shape" class="form-control" v-model="form.mane_shape">
						<option selected disabled :value="null">-- Select a Gene --</option>
						<option v-for="shape in maneshapes" :value="shape.id">{{ shape.name }}</option>
					</select>
					<!-- mane color -->
					<label for="mane_color" class="mt-2 mb-0">Mane Color</label>
					<select name="mane_color" class="form-control" v-model="form.mane_color">
						<option selected disabled :value="null">-- Select a Gene --</option>
						<option v-for="color in manecolors" :value="color.id">{{ color.name }}</option>
					</select>
				</div>
				<div class="col col-lg-6 col-md-6 col-sm-12 col-12">
					<!-- markings -->
					<label for="markings" class="mt-2 mb-0">Markings</label>
				</div>
			</div>
		</form>
	</div>
</template>

<script>
	export default {
		props: ['bases', 'eyes', 'maneshapes', 'manecolors'],

		data()
		{
			return {
				form: {
					base_color: null,
					eye_color: null,
					mane_shape: null,
					mane_color: null,
					markings: [],
				},
				price:
				{
					sb: 0,
					gb: 0,
				}
			}
		},

		methods:
		{
			/**
			 * Submit the form data.
			 *
			 * @return void
			 */
			submit()
			{
				// set _self
				var _self = this;

				// send request
				axios.post('/liomart', this.form).then(function(response)
				{
					// set variables
					_self.price.sb = response.data.sb_price;
					_self.price.gb = response.data.gb_price;
				});
			},
		},

		watch:
		{
			'form':
			{
				handler: function()
				{
					this.submit();
				},
				deep: true,
			}
		}
	}
</script>