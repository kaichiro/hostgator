<template>
  <div>
    <div id="main">
      <div class="container">
        <div class="row">
          <div class="col-md">
            <AppItemList
              v-bind:items="prefixes"
              title="Prefixos"
              v-on:addItem="addPrefix"
              v-on:deleteItem="deletePrefix"
            />
          </div>
          <div class="col-md">
            <AppItemList
              v-bind:items="sufixes"
              title="Sufixos"
              v-on:addItem="addSufix"
              v-on:deleteItem="deleteSufix"
            />
          </div>
        </div>
        <br />
        <h5>
          Domínios
          <span class="badge badge-info">{{domains.length}}</span>
        </h5>
        <div class="card">
          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item" v-for="domain in domains" v-bind:key="domain.name">
                <div class="row">
                  <div class="col-md">{{domain.name}}</div>
                  <div class="col-md text-right">
                    <a class="btn btn-info" v-bind:href="domain.checkout" target="_blank">
                      <span class="fa fa-shopping-cart"></span>
                    </a>
                  </div>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.css";
import "font-awesome/css/font-awesome.css";
import AppItemList from "./AppItemList";

export default {
	name: "DomainList",
	components: {
		AppItemList
	},
	data: function() {
		return {
			prefixes: ["Air", "Jet", "Flight"],
			sufixes: ["Hub", "Station", "Mart"]
		};
	},
	methods: {
		addPrefix(pPrefix) {
			const prefix = pPrefix.trim();
			if (prefix && prefix.length > 0) {
				this.prefixes.push(prefix);
			}
		},
		addSufix(pSufix) {
			const sufix = pSufix.trim();
			if (sufix && sufix.length > 0) {
				this.sufixes.push(sufix);
			}
		},
		deletePrefix(prefix) {
			this.prefixes.splice(this.prefixes.indexOf(prefix), 1);
		},
		deleteSufix(sufix) {
			this.sufixes.splice(this.sufixes.indexOf(sufix), 1);
		}
	},
	computed: {
		domains() {
			const domains = [];
			for (const prefix of this.prefixes) {
				for (const sufix of this.sufixes) {
					const name = prefix + sufix;
					const url = name.toLowerCase();
					const checkout = `https://checkout.hostgator.com.br/?a=add&sld=${url}&tld=.com.br`;
					domains.push({ name, checkout });
				}
			}
			return domains;
		}
	}
};
</script>

<style>
</style>
