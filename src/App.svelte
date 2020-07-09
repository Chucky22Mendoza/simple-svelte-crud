<script>

	import { v4 } from 'uuid';
	import Noty from 'noty';
	import 'noty/lib/noty.css';
	import 'noty/lib/themes/sunset.css'

	let products = [
		{
			id: 1,
			name: "HP Pavilion Notebook",
			description: "HP Laptop",
			category: "laptops",
			imageURL: "https://www8.hp.com/lamerica_nsc_cnt_amer/es/images/i_05_hp_pavilion_15_tcm237_2229708_tcm237_2229709_tcm237-2229708.jpg",
		},
		{
			id: 2,
			name: "Mouse Razer",
			description: "Gaming Mouse",
			category: "peripherials",
			imageURL: "https://lh3.googleusercontent.com/proxy/1OzDutdk_bbUgIXKwdn41sH7lRjtzrb-IV3tBVS68mPKd2k_Z9UqqrNH8bGXRH5rNVGzmBZWIIR5OQzO_Trow6CQxqifrWoDaiermmraJP8CJJZTEmLGCJZgu9Thg29OmK_K3HINzxU8a48d2Eow5bhf-q-feRaO_CSFTZwpy7VBcm20b_Uhvu2lTReKZtOHE89qrfM4D8KFbsL4GtBFPQk",
		}
	];

	let product = {
		id: "",
		name: "",
		description: "",
		category: "",
		imageURL: "",
	};

	let editStatus = false;

	const cleanProduct = () => {
		product = {
			id: "",
			name: "",
			description: "",
			category: "Select a Category",
			imageURL: "",
		};
	};

	const addProduct = () => {
		const newProduct = {
			id: v4(),
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL
		};

		products = products.concat(newProduct);
		cleanProduct();
		new Noty({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			layout: 'topCenter',
			text: 'Product Added Successfully'
		}).show();
	};

	const updateProduct = () => {
		let updatedProduct = {
			id: product.id,
			name: product.name,
			description: product.description,
			category: product.category,
			imageURL: product.imageURL
		};

		const productIndex = products.findIndex(p => p.id === product.id);
		products[productIndex] = updatedProduct;
		cleanProduct();
		editStatus = false;
		new Noty({
			theme: 'sunset',
			type: 'success',
			timeout: 3000,
			layout: 'topCenter',
			text: 'Product Updated Successfully'
		}).show();
	};

	const onSubmitHandler = e => {
		if (!editStatus) {
			addProduct();
		} else {
			updateProduct();
		}
	};

	const deleteProduct = (id) => {
		products = products.filter(product => product.id !== id);
		new Noty({
			theme: 'sunset',
			type: 'error',
			timeout: 3000,
			layout: 'topCenter',
			text: 'Product Deleted Successfully'
		}).show();
	};

	const editProduct = (productEdited) => {
		product = productEdited;
		editStatus = true;
	};
</script>

<main>
	<div class="container pt-5">
		<div class="row">
			<div class="col-md-6">
				{#each products as product}
					<div class="card mb-2">
						<div class="row">
							<div class="col-md-4">
								{#if !product.imageURL}
									<img src="images/noproducts.png" alt="" class="img-fluid p-3 ml-4">
								{:else}
									<img src="{product.imageURL}" alt="" class="py-4 pl-4" width="100%">
								{/if}
							</div>
							<div class="col-md-8">
								<div class="card-body">
									<h5>
										<strong>{product.name}</strong>
										<span class="badge badge-primary">
											<small>{product.category}</small>
										</span>
									</h5>
									<p class="card-text">{product.description}</p>

									<button on:click={deleteProduct(product.id)} class="btn btn-danger">
										Delete
									</button>
									<button on:click={editProduct(product)} class="btn btn-primary">
										Edit
									</button>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
			<div class="col-md-6">
				<div class="card">
					<div class="card-body">
						<form on:submit|preventDefault={onSubmitHandler}>

							<div class="form-group">
								<input
								bind:value={product.name}
								type="text"
								placeholder="Product Name..."
								id="product-name"
								class="form-control"
								required/>
							</div>

							<div class="form-group">
								<textarea
								bind:value={product.description}
								id="product-description"
								rows="3"
								placeholder="Product description..."
								class="form-control"
								required/>
							</div>

							<div class="form-group">
								<input
								bind:value={product.imageURL}
								type="url"
								id="product-image-url"
								placeholder="https://loginlock-portfolio.js.org"
								class="form-control"/>
							</div>

							<div class="form-group">
								<select
									bind:value={product.category}
									id="category"
									class="form-control"
									required>
										<option selected disabled>Select a Category</option>
										<option value="laptops">Laptops</option>
										<option value="peripherials">Peripherials</option>
										<option value="servers">Servers</option>
								</select>
							</div>

							<button class="btn btn-secondary btn-block">
								{#if !editStatus}
									Save Product
								{:else}
									Update Product
								{/if}
							</button>
						</form>
					</div>
				</div>
			</div>
		</div>
	</div>
</main>

<style>
</style>