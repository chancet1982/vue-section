<template >
  <section :class="['section', sectionClass,  isValid ? 'valid': 'invalid' , expanded ? 'expanded' : 'collapsed', {'has-header': hasHeader},{'has-footer': hasFooter}]">
	<header :class="['section-header', {'togglable': togglableOnHeader}]" @click="toggleSection" v-if="hasHeader">
      <slot name="section-header"></slot>
	  <i :class="['section-header-icon', {'togglable': togglableOnIcon} ]" v-if="togglable"><slot name="section-header-icon"><svg viewBox="0 0 16 16" xmlns="http://www.w3.org/2000/svg" fill-rule="evenodd" clip-rule="evenodd" stroke-linejoin="round" stroke-miterlimit="1.414"><g fill="#374355"><path d="M13.43 7.144c.247.215.376.535.348.86-.028.328-.21.62-.49.79l-.033.02c-.38.23-.867.19-1.206-.1L8 5.25 3.955 8.71c-.34.29-.83.332-1.214.1l-.013-.008c-.284-.172-.468-.47-.497-.8-.028-.33.103-.654.354-.87L8 2.43l5.43 4.714z"/><path d="M13.43 11.872c.247.214.376.535.348.86-.028.328-.21.62-.49.79l-.033.02c-.38.23-.867.19-1.206-.1L8 9.978 3.955 13.44c-.34.29-.83.332-1.214.1l-.013-.008c-.284-.172-.468-.47-.497-.8-.028-.33.103-.654.354-.872L8 7.16l5.43 4.712z"/></g></svg></slot></i>
	</header>
    <main class='section-main'>
		<div class="section-main-padding">
      		<slot></slot>
		</div>
    </main>
    <footer class='section-footer' v-if="hasFooter">
		<div class="section-footer-padding">
	  	  <slot name="section-footer"></slot>
		</div>
    </footer>
  </section>
</template>
<script>
export default {
  name: 'section',
	props: {
		isValid:{
			required: false,
			type: Boolean,
			default: true,
		},
		isExpanded:{
			required: false,
			type: Boolean,
			default: true,
		},
		isTogglable: {
			required: false,
			type: Boolean,
			default: true,
		},
		toggleOnHeader: {
			required: false,
			type: Boolean,
			default: true,
		},
		sectionClass: {
			required: false,
			type: String,
			default: () => {
				return "";
			}
		},
		isExternallyControlled: {
			required: false,
			type: Boolean,
			default: false,
		},
	},
	computed : {
		hasHeader () {
		  return !!this.$slots['section-header'];
		},
		hasFooter () {
		  return !!this.$slots['section-footer'];
		},
		togglableOnHeader () {
		  return this.togglable && this.toggleOnHeader;
		},
		togglableOnIcon () {
		  return this.togglable && !this.toggleOnHeader;
		},
		expanded (){
			return this.isExternallyControlled ? this.isExpanded : this.expandedData;
		},
		togglable (){
			return this.isTogglable && this.hasHeader;
		}
	},
	data (){
		return {
			expandedData: this.isExpanded,
		}
	},
	methods: {
		toggleSection() {
			if (this.togglable && !this.isExternallyControlled) {
				this.expandedData = !this.expandedData;
			}
		},
	},
}
</script>

<style lang="scss" scoped>
	$section-bg: #fff;
	$section-border-color: darken($section-bg,10%);
	$section-header-bg: darken($section-bg,3%);
	$section-footer-bg: darken($section-bg,3%);
	$section-header: 32px;
	$section-icon-size: 16px;
	$main-padding: 20px;

	section {
		margin-top: 10px;
		&:first-of-type {
			margin-top: 0px;
		}
	}

	.section {
		background: $section-bg;
		box-shadow: 0 1px 2px 0 rgba(0,0,0,0.1);
		> .section-main,
		> .section-footer {
			overflow: hidden;
			box-sizing: border-box;
			-webkit-transition: all 0.6s ease ;
			transition: all 0.6s ease ;
			> .section-main-padding,
			> .section-footer-padding {
				padding: $main-padding;
			}
		}
		> .section-footer {
			background:$section-header-bg;
			border-top: 1px solid $section-border-color;			
		}
		> .section-header {
			position: relative;
			background:$section-header-bg;
			border-bottom: 1px solid $section-border-color;
			height: $section-header;
			line-height: $section-header;
			padding-right: $section-header;
			padding-left: $main-padding;
			&.togglable {
				cursor: pointer;
			}
			i {
				-webkit-transition: all .3s ease ;
				transition: all .3s ease ;
				position: absolute;
				top:0;
				right: 0;
				width: $section-header;
				height: $section-header;
				&.togglable {
					cursor: pointer;
				}
				svg {
					width: $section-icon-size;
					height: $section-icon-size;
					position: absolute;
					top:50%;
					left: 50%;
					transform: translate(-50%,-50%);
				}
			}
		}
		&.collapsed {
			> .section-main,
			> .section-footer {
				max-height: 0;
			}
		}
		&.expanded {
			> .section-main,
			> .section-footer {
				max-height: 640px;
			}
			> .section-header i{
				-webkit-transform: rotate(180deg) ;
			    transform: rotate(180deg) ;
			}
		}
	}
</style>
