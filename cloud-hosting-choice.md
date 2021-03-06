Servers hosted on:

* [OVH](https://www.ovh.com/) — constantly running build servers.
* [Vultr](https://www.vultr.com/?ref=7263602) — build servers on demand (reserve).
* [UpCloud](https://www.upcloud.com/register/?promo=Z78TBU) — router and other control services. 

(links are [referral](https://www.upcloud.com/blog/join-our-referral-program/)).

## Scaleway

Scaleway simply sucks. 

* No way to install custom OS (no iPXE or ISO support), prebuilt images are outdated, poor choice anyway (no not only RancherOS, but also CoreOS).
* And company doesn't respond to answers and comments. There are number of public repositories, but not [maintained](https://github.com/scaleway-community/scaleway-coreos/issues/1#issuecomment-347016327) and outdated.
* A lot of pitfalls and critical issues [without solutions](https://github.com/scaleway/image-ubuntu/issues/87) for months.

## Vultr

In general, good. There are number of minor issues, that's make Vultr not so awesome compared to DigitalOcean:

* Snapshot per the whole disk, as result, very slow. At least 3 minutes is required to restore (25 GB SSD). So, this feature is not usable at all, and it is more suitable just create a new server from scratch using boot script (~45 seconds).
* Not easy to install CoreOS because at least 2 GB RAM is required, and provided image is outdated. Solution? Just install provided image and upgrade (adds ~10 seconds to setup new server).

## OVH

VPS Cloud.

* No sexy UI (but UI is still fully functional and quite usable).
* No explicit ISO or iPXE support.
* ~3 minutes to install, then ~3 minutes to boot in Resque mode to install custom OS.
* No hourly billing. 

But NVMe disks (and as result, superior performance), ability to install any OS using Resque mode. What else do you want?

## UpCloud

UpCloud is great. Really great. It is able even to gracefully shutdown server from admin panel.

The only issue why it is not a winner — price. For 20$ on Vultr you will get 4 GB RAM. On UpCloud only 2 GB. Because of lzma compression, for one build task more than 1 GB RAM is required. So, UpCloud server will be twice as expensive (40$).

## DigitalOcean

Well... CPU as Vultr, price and RAM as UpCloud. So, no reason to use it. Benchmark "build AppImage and deb" was not performed, because results of "build AppImage" is enough to say that UpCloud is a winner (again, Vultr offers you twice more memory for the same price).

## Linode

Linode was good 5 years ago, but now no reason even try to use it and do benchmarks. Anyway, see why [Linode was rejected](https://github.com/develar/electron-build-service/issues/3#issuecomment-349280483).

## Benchmarks

Ok, Scaleway sucks, but maybe 14$ for 4 Atom CPU and 8 GB RAM is a good reason to overcome all issues (e.g. use Ubuntu instead of CoreOS, take a risk to use outdated and not tested Linux Kernel to be able reboot server)?
Well, to build AppImage (gzip, not CPU hungry) and deb (CPU hungry because of xz compression using 7z):
* Vultr: 81s 43ms (20$)
* OVH: 59s 668ms (VPS Cloud 2, 19$ or 17$ if pay per year)
* Scaleway: 111s 913ms (C2S)

Why? Because Atom CPU is slow compared to 1 vCPU on Vultr/UpCloud. Of course, xz must be not used because slow and badly implemented (not really multi-threaded), only 7z must be, but still. So, 4 Atom CPU for build task is not so good as 2 vCPU even if you use modern decent multi-cpu aware software like 7zip.

So, even if Vultr costs 20$ (not 14$) and offers 4 GB RAM instead of 8 GB RAM, Vultr/OVH is a winner.

Yes, UpCloud vCPU is more powerful compared to Vultr vCPU, but RAM not enough. It is a reason why Vultr 10$ plans is not used, only 20$+ — build job concurrency equals to cpuCount + 1. So, for 10$ server it means that there is a chance that second job will fail with out of memory (1 vCPU and 2 GB RAM).