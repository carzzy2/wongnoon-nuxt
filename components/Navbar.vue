<template>
  <div>
    <b-navbar toggleable="lg" type="dark" variant="warning">
      <b-navbar-brand href="/">
        <img src="~/assets/wongnoon-logo.png" height='60' />
      </b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">

        </b-navbar-nav>
      </b-collapse>
    </b-navbar>
  </div>
</template>
<script>
export default {
  name: 'BooststrapNav',
}
</script>
