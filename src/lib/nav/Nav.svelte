<script lang="ts">
  import { page } from "$app/stores";
  import NavItem from "./NavItem.svelte";
  import { onDestroy } from "svelte";
  let isAdmin = false;
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div id="nav">
	<NavItem url="/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M10.55 2.533a2.25 2.25 0 0 1 2.9 0l6.75 5.695c.508.427.8 1.056.8 1.72v9.802a1.75 1.75 0 0 1-1.75 1.75h-3a1.75 1.75 0 0 1-1.75-1.75v-5a.75.75 0 0 0-.75-.75h-3.5a.75.75 0 0 0-.75.75v5a1.75 1.75 0 0 1-1.75 1.75h-3A1.75 1.75 0 0 1 3 19.75V9.947c0-.663.292-1.292.8-1.72l6.75-5.694Z" fill="#ffffff"/></svg>
		Home
	</NavItem>
	<NavItem url="/firewall/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M10 16h4a1 1 0 0 1 .117 1.993L14 18h-4a1 1 0 0 1-.117-1.993L10 16h4-4Zm-2-5h8a1 1 0 0 1 .117 1.993L16 13H8a1 1 0 0 1-.117-1.993L8 11h8-8ZM5 6h14a1 1 0 0 1 .117 1.993L19 8H5a1 1 0 0 1-.117-1.993L5 6h14H5Z" fill="#ffffff"/></svg>
		Firewall
	</NavItem>
	<NavItem url="/dns/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M8.904 16.5H12v.5c0 1.728.59 3.072 1.383 4.09-.439.584-.912.91-1.383.91-1.197 0-2.401-2.095-3.038-5.205l-.058-.294Zm-.23-1.499H12v-1.5c0-.85.676-1.453 1.409-1.52.773-.07 1.484-.33 2.084-.64a25.619 25.619 0 0 0-.069-1.34H8.575l-.033.488A25.982 25.982 0 0 0 8.674 15Zm13.127-5c.124.611.193 1.243.2 1.89-1.33-.287-2.416-1.073-2.965-1.544a1.576 1.576 0 0 0-.707-.346h3.472ZM3.065 16.5h4.306c.365 2.082.983 3.854 1.792 5.093a10.028 10.028 0 0 1-5.951-4.814l-.147-.279Zm-.867-6.5h4.87a28.205 28.205 0 0 0 .033 4.42l.058.58H2.456a10.048 10.048 0 0 1-.258-5Zm12.746-7.424-.108-.17A10.027 10.027 0 0 1 21.372 8.5H16.78c-.316-2.416-.957-4.492-1.837-5.923ZM9.04 2.444l.122-.037c-.88 1.35-1.535 3.329-1.883 5.654l-.061.44H2.628A10.028 10.028 0 0 1 9.04 2.443ZM12 2.002c1.318 0 2.645 2.542 3.214 6.183l.046.315H8.74c.54-3.809 1.904-6.498 3.26-6.498Zm10.5 10.975c-1.907-.172-3.434-1.287-4.115-1.87a.601.601 0 0 0-.77 0c-.682.583-2.21 1.698-4.116 1.87a.538.538 0 0 0-.5.523V17c0 4.223 4.094 5.716 4.873 5.962a.42.42 0 0 0 .255 0c.78-.246 4.872-1.74 4.872-5.962v-3.5a.538.538 0 0 0-.5-.523Z" fill="#ffffff"/></svg>
		Sperren & DNS
	</NavItem>
	<NavItem url="/vpn/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M3 5.75V11c0 5.001 2.958 8.676 8.725 10.948a.75.75 0 0 0 .55 0C18.042 19.676 21 16 21 11V5.75a.75.75 0 0 0-.75-.75c-2.663 0-5.258-.943-7.8-2.85a.75.75 0 0 0-.9 0C9.008 4.057 6.413 5 3.75 5a.75.75 0 0 0-.75.75ZM13.995 11a2 2 0 0 1-1.245 1.852v2.398a.75.75 0 0 1-1.5 0v-2.394A2 2 0 1 1 13.995 11Z" fill="#ffffff"/></svg>
		Tunnel
	</NavItem>
	<NavItem url="/vlans/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M8.904 16.5h6.192C14.476 19.773 13.234 22 12 22c-1.197 0-2.4-2.094-3.038-5.204l-.058-.294h6.192-6.192Zm-5.838.001H7.37c.365 2.082.983 3.854 1.793 5.093a10.029 10.029 0 0 1-5.952-4.814l-.146-.279Zm13.563 0h4.305a10.028 10.028 0 0 1-6.097 5.093c.755-1.158 1.344-2.778 1.715-4.681l.076-.412h4.306-4.306Zm.302-6.5h4.87a10.055 10.055 0 0 1-.257 5H16.84a28.539 28.539 0 0 0 .13-4.344L16.93 10h4.87-4.87ZM2.198 10h4.87a28.211 28.211 0 0 0 .034 4.42l.057.58H2.456a10.047 10.047 0 0 1-.258-5Zm6.377 0h6.85a25.838 25.838 0 0 1-.037 4.425l-.062.575H8.674a25.979 25.979 0 0 1-.132-4.512L8.575 10h6.85-6.85Zm6.37-7.424-.109-.17A10.027 10.027 0 0 1 21.372 8.5H16.78c-.316-2.416-.956-4.492-1.837-5.923l-.108-.17.108.17Zm-5.903-.133.122-.037C8.283 3.757 7.628 5.736 7.28 8.06l-.061.44H2.628a10.028 10.028 0 0 1 6.414-6.057l.122-.037-.122.037ZM12 2.002c1.319 0 2.646 2.542 3.214 6.183l.047.315H8.739C9.28 4.691 10.643 2.002 12 2.002Z" fill="#ffffff"/></svg>
		Netzwerke
	</NavItem>
	<NavItem url="/devices/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M2.75 16.5h18.5a.75.75 0 0 1 .101 1.493L21.25 18H2.751a.75.75 0 0 1-.102-1.494l.102-.006h18.498H2.751ZM18.25 5c.966 0 1.75.784 1.75 1.75v7.5A1.75 1.75 0 0 1 18.25 16H5.75A1.75 1.75 0 0 1 4 14.25v-7.5C4 5.784 4.783 5 5.75 5h12.5Z" fill="#ffffff"/></svg>
		Geräte
	</NavItem>
  <!-- div is used to get the navitems after it to the bottom -->
  <div id="nav-bottom"></div>
	<NavItem url="/controller/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M10.537 2.534a2.25 2.25 0 0 1 2.903-.002L20.2 8.23c.507.427.8 1.057.8 1.72v9.799a1.75 1.75 0 0 1-1.75 1.75H4.75A1.75 1.75 0 0 1 3 19.75v-9.8c0-.662.292-1.29.797-1.718l6.74-5.698Zm5.247 9.243a.75.75 0 1 0-1.061-1.06l-3.973 3.972-1.47-1.47a.75.75 0 1 0-1.06 1.061l2 2a.75.75 0 0 0 1.06 0l4.504-4.503Z" fill="#ffffff"/></svg>
		Controller
	</NavItem>
  <NavItem url="/settings/">
		<svg width="24" height="24" fill="none" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M12.012 2.25c.734.008 1.465.093 2.182.253a.75.75 0 0 1 .582.649l.17 1.527a1.384 1.384 0 0 0 1.927 1.116l1.401-.615a.75.75 0 0 1 .85.174 9.792 9.792 0 0 1 2.204 3.792.75.75 0 0 1-.271.825l-1.242.916a1.381 1.381 0 0 0 0 2.226l1.243.915a.75.75 0 0 1 .272.826 9.797 9.797 0 0 1-2.204 3.792.75.75 0 0 1-.848.175l-1.407-.617a1.38 1.38 0 0 0-1.926 1.114l-.169 1.526a.75.75 0 0 1-.572.647 9.518 9.518 0 0 1-4.406 0 .75.75 0 0 1-.572-.647l-.168-1.524a1.382 1.382 0 0 0-1.926-1.11l-1.406.616a.75.75 0 0 1-.849-.175 9.798 9.798 0 0 1-2.204-3.796.75.75 0 0 1 .272-.826l1.243-.916a1.38 1.38 0 0 0 0-2.226l-1.243-.914a.75.75 0 0 1-.271-.826 9.793 9.793 0 0 1 2.204-3.792.75.75 0 0 1 .85-.174l1.4.615a1.387 1.387 0 0 0 1.93-1.118l.17-1.526a.75.75 0 0 1 .583-.65c.717-.159 1.45-.243 2.201-.252ZM12 9a3 3 0 1 0 0 6 3 3 0 0 0 0-6Z" fill="#ffffff"/></svg>
    Einstellungen
  </NavItem>
</div>

<style>
  #nav {
    background-color: rgba(29, 29, 29, 0.5);
    backdrop-filter: blur(15px);
    padding: 10px;
    padding-left: 5px;
    display: flex;
    flex-direction: column;
    width: 20%;
    border: 2px solid rgba(46, 46, 46, 0.8);
    border-left: none;
    border-top-right-radius: 10px;
    border-bottom-right-radius: 10px;
    height: 85vh;
    animation: slideIn 800ms forwards cubic-bezier(0.83, 0, 0.17, 1);
    z-index: 999;
    view-transition-name: nav;
  }

  #nav-bottom {
    flex-grow: 1;
  }

  @keyframes slideIn {
    from {
      transform: translateX(-100%);
    }
    to {
      transform: translateX(0%);
    }
  }

  @keyframes slideOut {
    from {
      transform: translateX(0%);
    }
    to {
      transform: translateX(-100%);
    }
  }

  @media(max-width: 900px) {
    #nav {
      position: fixed;
      /* display: none; */
      width: 80%;
    }
    .closed {
      display: none !important;
    }
    .closing {
      animation: slideOut 800ms forwards ease-in-out !important;
    }
  }
</style>