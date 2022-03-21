<script>
	import PepCheck from "./PepCheck.svelte";
	import Employees from "./EmployeesCompany.svelte";
	import Company from "./Company.svelte";
	import EmployeesCompany from "./EmployeesCompany.svelte";
	import { navn } from './store.js';

	let name;
	navn.subscribe(navn => {
		name = navn;
	});
</script>

<main>
	<h1>{name}</h1>
	<PepCheck/>
	<Employees/>
	<Company/>
	<EmployeesCompany/>
</main>

<style>
	
</style>