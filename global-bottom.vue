<template>
  <footer 
		class="absolute bottom-0 left-0 right-0 py-3 px-15"	
	>
		<div class="float-left">
		<img class="w-30" src="https://www.costrategix.com/wp-content/themes/understrap-child/images/logo.svg" alt="">
		</div>

		<div class="float-right" v-if="$slidev.nav.currentPage != 1">
    {{ $slidev.nav.currentPage }} / {{ $slidev.nav.total + 1 }}
		</div>

  </footer>  
</template>