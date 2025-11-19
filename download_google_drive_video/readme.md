để download google drive video thì mở inpect tool chọn network và filter "playback" ở XHR
đôi lúc sẽ ko hiện ra thì cứ reload lại page, tắt mở devtool vài lần thì playback sẽ hiện ra json

khi hiện ra json thì có nhiều chất lượng, tạm thời mình chọn 1080 thì nó sẽ như này
 {
                    "itag": 37,
                    "url": "https://rr5---sn-npoe7nsr.c.drive.google.com/videoplayback?expire=1760611468&ei=XKLwaOGtGqOeo-wP9q6AwQI&ip=171.240.253.199&id=670e9de2144b84b8&itag=37&source=webdrive&requiressl=yes&xpc=EghonaK1InoBAQ==&met=1760610668,&mh=6Q&mm=32,26&mn=sn-npoe7nsr,sn-ogul7n76&ms=su,onr&mv=m&mvi=5&pl=21&rms=su,su&ttl=transient&susc=dr&obr=https://drive.google.com&svpuc=1&driveid=1_BlKCDFTxXRmC6W0LOulWrYNzsrWmZ-K&app=explorer&eaua=Bx6icfMFJ1A&mime=video/mp4&vprv=1&prv=1&rqh=1&cnr=14&dur=7524.403&lmt=1760074229446259&mt=1760600215&fvip=3&subapp=DRIVE_WEB_FILE_VIEWER&txp=0016224&sparams=expire,ei,ip,id,itag,source,requiressl,xpc,ttl,susc,obr,svpuc,driveid,app,eaua,mime,vprv,prv,rqh,cnr,dur,lmt&sig=AJfQdSswRQIhALpJ84WWYACN5kmPNR-fY0lSH-vo4cm9idc5AyJeeTHjAiAiRBllLpN5CsQY8mVy9vp_tQrlz18uawVu-W307q_c9g==&lsparams=met,mh,mm,mn,ms,mv,mvi,pl,rms&lsig=APaTxxMwRQIhAJ9ZvU4n4I2_Vx_2oPhXXNEPD2OrgVSzQvXy4JXhBAtVAiBYkKZ5Y4O0zrvsadZ1WTUWMoESuObLd_NOsS1oEYaoQ==",
                    "transcodeMetadata": {
                        "mimeType": "video/mp4",
                        "width": 1920,
                        "height": 1080,
                        "contentLength": "2177825",
                        "approxDuration": "7524.403s",
                        "maxContainerBitrate": 397882,
                        "videoFps": 24,
                        "videoCodecString": "avc1.640032",
                        "audioCodecString": "mp4a.40.2"
                    }
                }

file index.html đã code sẳn dán json trả về vào đó thì sẽ xuất ra đúng link video để tải về
