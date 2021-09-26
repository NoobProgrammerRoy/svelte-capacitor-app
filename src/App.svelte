<script>
	import Router from 'svelte-spa-router';
	import Header from './Components/Header.svelte';
	import Footer from './Components/Footer.svelte'
	import _pokemon from './Pages/_pokemon.svelte';
	import _items from './Pages/_items.svelte';
	import _moves from './Pages/_moves.svelte';
	import _about from './Pages/_about.svelte';
</script>

<!-- Header -->
<Header />

<!-- Router for different pages -->
<main>
	<Router routes={{
		'/': _pokemon,
		'/items': _items,
		'/moves': _moves,
		'/about': _about
	}} />
</main>

<!-- Footer -->
<Footer />