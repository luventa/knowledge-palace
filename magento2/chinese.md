php bin\magento setup:static-content:deploy zh_Hans_CN （ 如果报错 在deploy 和zh_Hans_CN之间加一个 -f 就行）
php bin\magento indexer:reindex 刷新索引
php bin\magento cache:clean 清空缓存
php bin\magento cache:flush  刷新缓存