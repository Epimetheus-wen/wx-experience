import md5 from 'js-md5';

// var reUrl = "https://test.callas2.caih.com" //测试
var reUrl = "https://api.caih.com" //生产

export function authorization() {
    let time = Math.round(new Date().getTime() / 1000).toString();
    console.log("sss")
    uni.getStorage({
        key: "phone",
        success: function(res) {
            if (res.data && res.data != undefined && res.data != "undefined") {
                console.log("phone===>", res.data)
                let phone = res.data
                wx.request({
                    url: reUrl + "/console/trialRequest.html?f=authorization",
                    data: {
                        nickname: phone
                    },
                    header: {
                        "content-type": "application/x-www-form-urlencoded",
                        sign: md5("DONGXINYITONG-ONLINE!" + time),
                        signtime: time,
                        phone: phone
                    },
                    method: "POST",
                    success(ress) {
                        console.log("授权")
                        console.log(ress.data);
                    }
                });
            }
        }
    });


}


export function getRequestCount(identifying, Phone) {
    return new Promise((resolve, reject) => {
        let time1 = Math.round(new Date().getTime() / 1000).toString();
        wx.request({
            url: reUrl + "/console/trialRequest.html?f=getRequestCount",
            data: {
                identifying: identifying,
            },
            header: {
                "content-type": "application/x-www-form-urlencoded",
                sign: md5("DONGXINYITONG-ONLINE!" + time1),
                signtime: time1,
                phone: Phone
            },
            method: "POST",
            success(response) {
                resolve(response.data);
            },
            fail(err) {
                reject(err);
            }
        })
    })
}