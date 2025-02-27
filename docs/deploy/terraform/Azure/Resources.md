# Terraform Azure resources

Here is the default output of `terraform plan`:

```bash
data.http.myip: Reading...
data.http.myip: Read complete after 0s [id=http://ipv4.icanhazip.com]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # azurerm_application_gateway.network will be created
  + resource "azurerm_application_gateway" "network" {
      + id                          = (known after apply)
      + location                    = "westus2"
      + name                        = "mage-data-prep-production-app-gateway"
      + private_endpoint_connection = (known after apply)
      + resource_group_name         = "mage-data-prep-production"

      + backend_address_pool {
          + fqdns        = []
          + id           = (known after apply)
          + ip_addresses = (known after apply)
          + name         = "mage-data-prep-production-backend-pool"
        }

      + backend_http_settings {
          + cookie_based_affinity               = "Disabled"
          + id                                  = (known after apply)
          + name                                = "mage-data-prep-production-http-setting"
          + pick_host_name_from_backend_address = false
          + port                                = 6789
          + probe_id                            = (known after apply)
          + protocol                            = "Http"
          + request_timeout                     = 60
          + trusted_root_certificate_names      = []
        }

      + frontend_ip_configuration {
          + id                            = (known after apply)
          + name                          = "mage-data-prep-production-frontend-ip-config"
          + private_ip_address            = (known after apply)
          + private_ip_address_allocation = "Dynamic"
          + private_link_configuration_id = (known after apply)
          + public_ip_address_id          = (known after apply)
        }

      + frontend_port {
          + id   = (known after apply)
          + name = "mage-data-prep-production-frontend-port"
          + port = 80
        }

      + gateway_ip_configuration {
          + id        = (known after apply)
          + name      = "mage-data-prep-production-ip-configuration"
          + subnet_id = (known after apply)
        }

      + http_listener {
          + frontend_ip_configuration_id   = (known after apply)
          + frontend_ip_configuration_name = "mage-data-prep-production-frontend-ip-config"
          + frontend_port_id               = (known after apply)
          + frontend_port_name             = "mage-data-prep-production-frontend-port"
          + host_names                     = []
          + id                             = (known after apply)
          + name                           = "mage-data-prep-production-listener"
          + protocol                       = "Http"
          + ssl_certificate_id             = (known after apply)
          + ssl_profile_id                 = (known after apply)
        }

      + request_routing_rule {
          + backend_address_pool_id    = (known after apply)
          + backend_address_pool_name  = "mage-data-prep-production-backend-pool"
          + backend_http_settings_id   = (known after apply)
          + backend_http_settings_name = "mage-data-prep-production-http-setting"
          + http_listener_id           = (known after apply)
          + http_listener_name         = "mage-data-prep-production-listener"
          + id                         = (known after apply)
          + name                       = "mage-data-prep-production-routing-rule"
          + priority                   = 10
          + redirect_configuration_id  = (known after apply)
          + rewrite_rule_set_id        = (known after apply)
          + rule_type                  = "Basic"
          + url_path_map_id            = (known after apply)
        }

      + sku {
          + capacity = 2
          + name     = "Standard_v2"
          + tier     = "Standard_v2"
        }

      + ssl_policy {
          + cipher_suites        = (known after apply)
          + disabled_protocols   = (known after apply)
          + min_protocol_version = (known after apply)
          + policy_name          = (known after apply)
          + policy_type          = (known after apply)
        }
    }

  # azurerm_container_group.container_group will be created
  + resource "azurerm_container_group" "container_group" {
      + exposed_port        = (known after apply)
      + fqdn                = (known after apply)
      + id                  = (known after apply)
      + ip_address          = (known after apply)
      + ip_address_type     = "Private"
      + location            = "westus2"
      + name                = "mage-data-prep-production"
      + network_profile_id  = (known after apply)
      + os_type             = "Linux"
      + resource_group_name = "mage-data-prep-production"
      + restart_policy      = "Always"
      + tags                = {
          + "Environment" = "production"
        }

      + container {
          + commands = (known after apply)
          + cpu      = 1
          + image    = "mageai/mageai:latest"
          + memory   = 1.5
          + name     = "mage-data-prep-production-container"

          + ports {
              + port     = 6789
              + protocol = "TCP"
            }

          + volume {
              + empty_dir            = false
              + mount_path           = "/home/src"
              + name                 = "mage-data-prep-fs"
              + read_only            = false
              + share_name           = "mage-data-prep-production-data"
              + storage_account_key  = (sensitive value)
              + storage_account_name = "magedataprepstorage"
            }
        }
    }

  # azurerm_network_profile.containergroup_profile will be created
  + resource "azurerm_network_profile" "containergroup_profile" {
      + container_network_interface_ids = (known after apply)
      + id                              = (known after apply)
      + location                        = "westus2"
      + name                            = "mage-data-prep-production-profile"
      + resource_group_name             = "mage-data-prep-production"

      + container_network_interface {
          + name = "mage-data-prep-production-nic"

          + ip_configuration {
              + name      = "aciipconfig"
              + subnet_id = (known after apply)
            }
        }
    }

  # azurerm_network_security_group.nsg-aci will be created
  + resource "azurerm_network_security_group" "nsg-aci" {
      + id                  = (known after apply)
      + location            = "westus2"
      + name                = "mage-data-prep-production-nsg-aci"
      + resource_group_name = "mage-data-prep-production"
      + security_rule       = [
          + {
              + access                                     = "Allow"
              + description                                = ""
              + destination_address_prefix                 = ""
              + destination_address_prefixes               = [
                  + "10.0.10.0/24",
                ]
              + destination_application_security_group_ids = []
              + destination_port_range                     = ""
              + destination_port_ranges                    = [
                  + "22",
                  + "443",
                  + "445",
                  + "6789",
                  + "8000",
                ]
              + direction                                  = "Inbound"
              + name                                       = "from-gateway-subnet"
              + priority                                   = 100
              + protocol                                   = "Tcp"
              + source_address_prefix                      = ""
              + source_address_prefixes                    = [
                  + "10.0.20.0/24",
                ]
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
          + {
              + access                                     = "Allow"
              + description                                = ""
              + destination_address_prefix                 = "*"
              + destination_address_prefixes               = []
              + destination_application_security_group_ids = []
              + destination_port_range                     = ""
              + destination_port_ranges                    = [
                  + "443",
                  + "445",
                  + "80",
                ]
              + direction                                  = "Outbound"
              + name                                       = "to-internet"
              + priority                                   = 100
              + protocol                                   = "Tcp"
              + source_address_prefix                      = "*"
              + source_address_prefixes                    = []
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
          + {
              + access                                     = "Deny"
              + description                                = ""
              + destination_address_prefix                 = "*"
              + destination_address_prefixes               = []
              + destination_application_security_group_ids = []
              + destination_port_range                     = "*"
              + destination_port_ranges                    = []
              + direction                                  = "Inbound"
              + name                                       = "DenyAllInBound-Override"
              + priority                                   = 900
              + protocol                                   = "*"
              + source_address_prefix                      = "*"
              + source_address_prefixes                    = []
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
          + {
              + access                                     = "Deny"
              + description                                = ""
              + destination_address_prefix                 = "*"
              + destination_address_prefixes               = []
              + destination_application_security_group_ids = []
              + destination_port_range                     = "*"
              + destination_port_ranges                    = []
              + direction                                  = "Outbound"
              + name                                       = "DenyAllOutBound-Override"
              + priority                                   = 900
              + protocol                                   = "*"
              + source_address_prefix                      = "*"
              + source_address_prefixes                    = []
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
        ]
    }

  # azurerm_network_security_group.nsg-public will be created
  + resource "azurerm_network_security_group" "nsg-public" {
      + id                  = (known after apply)
      + location            = "westus2"
      + name                = "mage-data-prep-production-nsg-public"
      + resource_group_name = "mage-data-prep-production"
      + security_rule       = [
          + {
              + access                                     = "Allow"
              + description                                = ""
              + destination_address_prefix                 = "*"
              + destination_address_prefixes               = []
              + destination_application_security_group_ids = []
              + destination_port_range                     = ""
              + destination_port_ranges                    = [
                  + "80",
                ]
              + direction                                  = "Inbound"
              + name                                       = "whitelist-inbound-ip"
              + priority                                   = 100
              + protocol                                   = "Tcp"
              + source_address_prefix                      = ""
              + source_address_prefixes                    = [
                  + "11.111.11.111/32",
                ]
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
          + {
              + access                                     = "Allow"
              + description                                = ""
              + destination_address_prefix                 = "*"
              + destination_address_prefixes               = []
              + destination_application_security_group_ids = []
              + destination_port_range                     = "65200-65535"
              + destination_port_ranges                    = []
              + direction                                  = "Inbound"
              + name                                       = "AllowInfraComms"
              + priority                                   = 200
              + protocol                                   = "Tcp"
              + source_address_prefix                      = "*"
              + source_address_prefixes                    = []
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
          + {
              + access                                     = "Deny"
              + description                                = ""
              + destination_address_prefix                 = "*"
              + destination_address_prefixes               = []
              + destination_application_security_group_ids = []
              + destination_port_range                     = "*"
              + destination_port_ranges                    = []
              + direction                                  = "Inbound"
              + name                                       = "DenyAllInBound-Override"
              + priority                                   = 900
              + protocol                                   = "*"
              + source_address_prefix                      = "*"
              + source_address_prefixes                    = []
              + source_application_security_group_ids      = []
              + source_port_range                          = "*"
              + source_port_ranges                         = []
            },
        ]
    }

  # azurerm_public_ip.public_ip will be created
  + resource "azurerm_public_ip" "public_ip" {
      + allocation_method       = "Static"
      + fqdn                    = (known after apply)
      + id                      = (known after apply)
      + idle_timeout_in_minutes = 4
      + ip_address              = (known after apply)
      + ip_version              = "IPv4"
      + location                = "westus2"
      + name                    = "mage-data-prep-production-public-ip"
      + resource_group_name     = "mage-data-prep-production"
      + sku                     = "Standard"
      + sku_tier                = "Regional"
    }

  # azurerm_resource_group.resource_group will be created
  + resource "azurerm_resource_group" "resource_group" {
      + id       = (known after apply)
      + location = "westus2"
      + name     = "mage-data-prep-production"
    }

  # azurerm_storage_account.aci_storage will be created
  + resource "azurerm_storage_account" "aci_storage" {
      + access_tier                       = (known after apply)
      + account_kind                      = "StorageV2"
      + account_replication_type          = "LRS"
      + account_tier                      = "Standard"
      + allow_nested_items_to_be_public   = true
      + cross_tenant_replication_enabled  = true
      + default_to_oauth_authentication   = false
      + enable_https_traffic_only         = true
      + id                                = (known after apply)
      + infrastructure_encryption_enabled = false
      + is_hns_enabled                    = false
      + large_file_share_enabled          = (known after apply)
      + location                          = "westus2"
      + min_tls_version                   = "TLS1_2"
      + name                              = "magedataprepstorage"
      + nfsv3_enabled                     = false
      + primary_access_key                = (sensitive value)
      + primary_blob_connection_string    = (sensitive value)
      + primary_blob_endpoint             = (known after apply)
      + primary_blob_host                 = (known after apply)
      + primary_connection_string         = (sensitive value)
      + primary_dfs_endpoint              = (known after apply)
      + primary_dfs_host                  = (known after apply)
      + primary_file_endpoint             = (known after apply)
      + primary_file_host                 = (known after apply)
      + primary_location                  = (known after apply)
      + primary_queue_endpoint            = (known after apply)
      + primary_queue_host                = (known after apply)
      + primary_table_endpoint            = (known after apply)
      + primary_table_host                = (known after apply)
      + primary_web_endpoint              = (known after apply)
      + primary_web_host                  = (known after apply)
      + public_network_access_enabled     = true
      + queue_encryption_key_type         = "Service"
      + resource_group_name               = "mage-data-prep-production"
      + secondary_access_key              = (sensitive value)
      + secondary_blob_connection_string  = (sensitive value)
      + secondary_blob_endpoint           = (known after apply)
      + secondary_blob_host               = (known after apply)
      + secondary_connection_string       = (sensitive value)
      + secondary_dfs_endpoint            = (known after apply)
      + secondary_dfs_host                = (known after apply)
      + secondary_file_endpoint           = (known after apply)
      + secondary_file_host               = (known after apply)
      + secondary_location                = (known after apply)
      + secondary_queue_endpoint          = (known after apply)
      + secondary_queue_host              = (known after apply)
      + secondary_table_endpoint          = (known after apply)
      + secondary_table_host              = (known after apply)
      + secondary_web_endpoint            = (known after apply)
      + secondary_web_host                = (known after apply)
      + shared_access_key_enabled         = true
      + table_encryption_key_type         = "Service"

      + blob_properties {
          + change_feed_enabled           = (known after apply)
          + change_feed_retention_in_days = (known after apply)
          + default_service_version       = (known after apply)
          + last_access_time_enabled      = (known after apply)
          + versioning_enabled            = (known after apply)

          + container_delete_retention_policy {
              + days = (known after apply)
            }

          + cors_rule {
              + allowed_headers    = (known after apply)
              + allowed_methods    = (known after apply)
              + allowed_origins    = (known after apply)
              + exposed_headers    = (known after apply)
              + max_age_in_seconds = (known after apply)
            }

          + delete_retention_policy {
              + days = (known after apply)
            }
        }

      + network_rules {
          + bypass                     = (known after apply)
          + default_action             = (known after apply)
          + ip_rules                   = (known after apply)
          + virtual_network_subnet_ids = (known after apply)

          + private_link_access {
              + endpoint_resource_id = (known after apply)
              + endpoint_tenant_id   = (known after apply)
            }
        }

      + queue_properties {
          + cors_rule {
              + allowed_headers    = (known after apply)
              + allowed_methods    = (known after apply)
              + allowed_origins    = (known after apply)
              + exposed_headers    = (known after apply)
              + max_age_in_seconds = (known after apply)
            }

          + hour_metrics {
              + enabled               = (known after apply)
              + include_apis          = (known after apply)
              + retention_policy_days = (known after apply)
              + version               = (known after apply)
            }

          + logging {
              + delete                = (known after apply)
              + read                  = (known after apply)
              + retention_policy_days = (known after apply)
              + version               = (known after apply)
              + write                 = (known after apply)
            }

          + minute_metrics {
              + enabled               = (known after apply)
              + include_apis          = (known after apply)
              + retention_policy_days = (known after apply)
              + version               = (known after apply)
            }
        }

      + routing {
          + choice                      = (known after apply)
          + publish_internet_endpoints  = (known after apply)
          + publish_microsoft_endpoints = (known after apply)
        }

      + share_properties {
          + cors_rule {
              + allowed_headers    = (known after apply)
              + allowed_methods    = (known after apply)
              + allowed_origins    = (known after apply)
              + exposed_headers    = (known after apply)
              + max_age_in_seconds = (known after apply)
            }

          + retention_policy {
              + days = (known after apply)
            }

          + smb {
              + authentication_types            = (known after apply)
              + channel_encryption_type         = (known after apply)
              + kerberos_ticket_encryption_type = (known after apply)
              + versions                        = (known after apply)
            }
        }
    }

  # azurerm_storage_share.container_share will be created
  + resource "azurerm_storage_share" "container_share" {
      + access_tier          = (known after apply)
      + enabled_protocol     = "SMB"
      + id                   = (known after apply)
      + metadata             = (known after apply)
      + name                 = "mage-data-prep-production-data"
      + quota                = 100
      + resource_manager_id  = (known after apply)
      + storage_account_name = "magedataprepstorage"
      + url                  = (known after apply)
    }

  # azurerm_subnet.sn-aci will be created
  + resource "azurerm_subnet" "sn-aci" {
      + address_prefixes                               = [
          + "10.0.10.0/24",
        ]
      + enforce_private_link_endpoint_network_policies = (known after apply)
      + enforce_private_link_service_network_policies  = (known after apply)
      + id                                             = (known after apply)
      + name                                           = "aci"
      + private_endpoint_network_policies_enabled      = (known after apply)
      + private_link_service_network_policies_enabled  = (known after apply)
      + resource_group_name                            = "mage-data-prep-production"
      + service_endpoints                              = [
          + "Microsoft.Storage",
        ]
      + virtual_network_name                           = "mage-data-prep-production"

      + delegation {
          + name = "acidelegationservice"

          + service_delegation {
              + actions = [
                  + "Microsoft.Network/virtualNetworks/subnets/join/action",
                  + "Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action",
                ]
              + name    = "Microsoft.ContainerInstance/containerGroups"
            }
        }
    }

  # azurerm_subnet.sn-public will be created
  + resource "azurerm_subnet" "sn-public" {
      + address_prefixes                               = [
          + "10.0.20.0/24",
        ]
      + enforce_private_link_endpoint_network_policies = (known after apply)
      + enforce_private_link_service_network_policies  = (known after apply)
      + id                                             = (known after apply)
      + name                                           = "public"
      + private_endpoint_network_policies_enabled      = (known after apply)
      + private_link_service_network_policies_enabled  = (known after apply)
      + resource_group_name                            = "mage-data-prep-production"
      + virtual_network_name                           = "mage-data-prep-production"
    }

  # azurerm_subnet_network_security_group_association.sn-nsg-aci will be created
  + resource "azurerm_subnet_network_security_group_association" "sn-nsg-aci" {
      + id                        = (known after apply)
      + network_security_group_id = (known after apply)
      + subnet_id                 = (known after apply)
    }

  # azurerm_subnet_network_security_group_association.sn-nsg-public will be created
  + resource "azurerm_subnet_network_security_group_association" "sn-nsg-public" {
      + id                        = (known after apply)
      + network_security_group_id = (known after apply)
      + subnet_id                 = (known after apply)
    }

  # azurerm_virtual_network.virtual_network will be created
  + resource "azurerm_virtual_network" "virtual_network" {
      + address_space       = [
          + "10.0.0.0/16",
        ]
      + dns_servers         = [
          + "10.0.0.4",
          + "10.0.0.5",
        ]
      + guid                = (known after apply)
      + id                  = (known after apply)
      + location            = "westus2"
      + name                = "mage-data-prep-production"
      + resource_group_name = "mage-data-prep-production"
      + subnet              = (known after apply)
      + tags                = {
          + "Environment" = "production"
        }
    }

Plan: 14 to add, 0 to change, 0 to destroy.

Changes to Outputs:
  + id = (known after apply)
  + ip = (known after apply)

─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if you run "terraform apply" now.
```
