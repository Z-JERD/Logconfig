## 日志的优先级从高到低依次为：FATAL、ERROR、WARN、INFO、DEBUG、TRACE

## Django中配置日志：djangolog
    1.在settings.py中添加djangolog文件中的settings中的内容
    2.如果不需要在控制台打印,就修改loggers中的内容， 将'handlers'中的'console'去掉
     'loggers': {
        'all': {
            'handlers': [file_handler'],  #不打印到控制台
            'level':'DEBUG',
            'propagate': True,
        },
     }
     3.如果要修改打印台输出的级别就修改 console中的level,如果为debug,则debug,info,error的信息均会在控制台输出
                                                         如果为info则info,error的信息会在控制台输出
         'handlers': {
                'console': {
                            'level': 'DEBUG',
                            'class': 'logging.StreamHandler',
                            'formatter': 'simple'
                        },
     4.修改日志文件中的输出级别：如果为debug则在default.log中会显示debug,info,error的信息,如果为info则default.log中会显示info,error的信息
          'file_handler': {
                'level': 'DEBUG',
                'class': 'logging.handlers.RotatingFileHandler',
                'filename': os.path.join(LOG_DIR, 'default.log')
                }
     5.将每天的日志放到一个文件中
        'filename': os.path.join(LOG_DIR, 'default-{date}.log'.format(date = time.strftime("%Y%m%d",time.localtime()))),

     6.在views中使用日记录
            from django.shortcuts import render, HttpResponse, redirect
            import logging
            logger = logging.getLogger('all')

            def index(request):
                logger.info("this is info")
                logger.debug("this is debug")
                logger.error("this is error")

                return HttpResponse('ok')


## 普通开发用到的日志：singletonlog
    1.控制台是否输出：
      #self.logger.addHandler(console_handler)

    2.修改记录日志的等级：
      self.logger.setLevel(logging.DEBUG)

    3.普通程序中调用日志;
        from singletonlog.logconfig import logger
            logger.info("this is info")
            logger.debug("this is debug")
            logger.error("this is error")

## Flask中配置日志
    1.控制打印台输出的等级 修改 root中的level 禁止在控制台打印 去掉handles中的console：
            "root": {
                "level": "INFO",
                "handlers": ["console", "info_file_handler", "error_file_handler"]

            }
        }

    2.修改记录的日志等级 修改info_file_handler中的level

        "info_file_handler": {
                "class": "logging.handlers.RotatingFileHandler",
                "level": "INFO",
                "formatter": "simple",
                "filename": "logger/default.log",
                "maxBytes": 10485760,
                "backupCount": 20,
                "encoding": "utf8"
            },

    3.调用日志:
        import logging
        from flask_log_config.config import *
        setup_logging()
        logger = logging.getLogger("root")
        if __name__ == '__main__':
            logger.info("this is a demo")
            logger.error("this is a error")
