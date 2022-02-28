<script>
	import { Router } from "@roxi/routify";
  	import { routes } from "../.routify/routes";

	const config = {
		queryHandler: {
			parse: search => fromEntries(new URLSearchParams(search)),
			stringify: params => '?' + (new URLSearchParams(params)).toString()
		},
		urlTransform: {
			apply: url => `/scoutle${url}`, //external URL
			remove: url => url.replace('/scoutle', ''), //internal URL
		},
		useHash: true
	};
</script>

<Router {routes} {config} />

<style lang="postcss" global>
	@tailwind base;
	@tailwind components;
	@tailwind utilities;
</style>