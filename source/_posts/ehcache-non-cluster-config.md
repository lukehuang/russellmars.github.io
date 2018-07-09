title: ehcache 非集群配置
date: 2015-09-01 11:54:42
tags: [java,ehcache]
categories: java
---
ehcache配置文件详解
<!--more-->
``` xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
	xsi:noNamespaceSchemaLocation="ehcache.xsd" 
	updateCheck="true" 
	monitoring="autodetect" 
	dynamicConfig="true">

    <diskStore path="java.io.tmpdir"/>
    <!--
    	默认缓存策略：
    	maxElementsInMemory：缓存最大个数。
    	eternal:对象是否永久有效，一但设置了，timeout将不起作用。
    	timeToIdleSeconds：设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是可闲置时间无穷大。 
    	timeToLiveSeconds：设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
    	overflowToDisk：当内存中对象数量达到maxElementsInMemory时，Ehcache将会对象写到磁盘中。
    	diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
    	maxElementsOnDisk：硬盘最大缓存个数。
    	diskPersistent：是否缓存虚拟机重启期数据，默认false
    	diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
    	memoryStoreEvictionPolicy：当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
    	clearOnFlush：内存数量最大时是否清除。
    -->
    <defaultCache 
    	maxElementsInMemory="10000" 
    	eternal="false" 
    	timeToIdleSeconds="30" 
    	timeToLiveSeconds="30" 
    	overflowToDisk="false"/>
    <!-- 
    	设置某具体缓存的缓存策略：
    	name：缓存名称
    	其他属性设置参考默认缓存策略

     -->
    <cache name="cache_token" 
        maxElementsInMemory="10000" 
        eternal="false"
        overflowToDisk="false" 
        timeToIdleSeconds="900" 
        timeToLiveSeconds="1800"
        memoryStoreEvictionPolicy="FIFO" />
 
</ehcache>
```

ehcache的工具类
``` java
public class CacheKit {
	
	private static CacheManager cacheManager;
	private static final Logger log = Logger.getLogger(CacheKit.class);
	
	static void init(CacheManager cacheManager) {
		CacheKit.cacheManager = cacheManager;
	}
	
	public static CacheManager getCacheManager() {
		return cacheManager;
	}
	
	static Cache getOrAddCache(String cacheName) {
		Cache cache = cacheManager.getCache(cacheName);
		if (cache == null) {
			synchronized(cacheManager) {
				cache = cacheManager.getCache(cacheName);
				if (cache == null) {
					log.warn("Could not find cache config [" + cacheName + "], using default.");
					cacheManager.addCacheIfAbsent(cacheName);
					cache = cacheManager.getCache(cacheName);
					log.debug("Cache [" + cacheName + "] started.");
				}
			}
		}
		return cache;
	}
	
	public static void put(String cacheName, Object key, Object value) {
		getOrAddCache(cacheName).put(new Element(key, value));
	}
	
	@SuppressWarnings("unchecked")
	public static <T> T get(String cacheName, Object key) {
		Element element = getOrAddCache(cacheName).get(key);
		return element != null ? (T)element.getObjectValue() : null;
	}
	
	@SuppressWarnings("rawtypes")
	public static List getKeys(String cacheName) {
		return getOrAddCache(cacheName).getKeys();
	}
	
	public static void remove(String cacheName, Object key) {
		getOrAddCache(cacheName).remove(key);
	}
	
	public static void removeAll(String cacheName) {
		getOrAddCache(cacheName).removeAll();
	}
}
```

## 参考文献
+ [Ehcache官网](http://ehcache.org/)
+ [Ehcache配置详解及CacheManager使用](http://www.cnblogs.com/sunxucool/p/3159076.html)