# internal_external_traffic
basic script that takes actions and IP addresses and returns the products that were used most externally

# How do you determine what product in Facebook was used most by the non-employee users for the last quarter? [Required parameters will be given]
library(dplyr)
product <- c(rep("messenger", 12), rep("shop", 12), rep("social.vr.dev", 8), rep("social.vr.prod", 8))
ip <- c("159.83.136.2", rep("external", 11), "159.83.136.2", "159.83.136.2", rep("external", 10), rep("159.83.136.2", 8), rep("external", 8))

df <- data.frame(product, ip)

ip.internal <- c("159.83.136.2")

# get the one with the most total logs for external

df %>% group_by(product) %>% summarise(ext.logs = count(ip))

df %>% filter(ip %nin% ip.internal) %>% count(product) %>% arrange(desc(n))

df %>% group_by(product) %>% summarise(n.total = n(), n.int = sum(ip %in% ip.internal), n.ext = sum(ip %nin% ip.internal)) %>%
  mutate(percent.ext = n.ext/n.total) %>% arrange(desc(percent.ext))
