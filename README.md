# hello

class FuWuReplyHandler(tornado.web.RequestHandler):
    def get(self,ru = ''):
        try:
            rid = self.get_argument('id',default='')
            if rid:
                cur = conn.cursor()
                cur.execute("select uname,content,dbdate,uid,forum_reply2.id,avatar from forum_reply2 left join user on user.id=forum_reply2.uid where rid = "+ rid +" order by forum_reply2.id desc limit 50")
                reviews = cur.fetchall()
                strtmp = ''
                avatar = ''
                for rs in reviews:
                    if rs[5]:
                        avatar = 'http://a.xczx.com/'+str(rs[5])+'.jpg!40'
                    else:
                        avatar = "http://static.xczx.com/main/images/avatar.png"
                    strtmp += '<div class="entry"><a href="https://www.xczx.com/u/'+ str(rs[3]) +'"><img src="'+avatar+'" class="avatar"></a><span class="user"><a href="https://www.xczx.com/u/'+ str(rs[3]) +'">'+ rs[0].encode('utf-8', 'ignore') +'</a></span><span class="date">'+ timedis(rs[2]) +'</span><p>'+ rs[1].encode('utf-8', 'ignore') +'</p></div>'
                self.write(strtmp)
        except Exception, e:
            strtmp = str(traceback.format_exc())
            self.write('<p id="error">' + strtmp + '</p>')
