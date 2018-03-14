# Customise this file, documentation can be found here:
# https://github.com/fastlane/fastlane/tree/master/fastlane/docs
# All available actions: https://docs.fastlane.tools/actions
# can also be listed using the `fastlane actions` command

# Change the syntax highlighting to Ruby
# All lines starting with a # are ignored when running `fastlane`

# If you want to automatically update fastlane if a new version is available:
# update_fastlane

# This is the minimum version number required.
# Update this, if you use features of a newer version
fastlane_version "2.26.1"

default_platform :ios

platform :ios do
  before_all do
    updateProjectBuildNumber
  end
  
  lane :beta do
    buildScheme = ""
    outputDir = "~/Documents/ipa/fir"
    outputName = "#{buildScheme}_#{Time.now.strftime("%Y%m%d%H%M%S")}.ipa"
    gym(
      output_directory: outputDir,
      scheme: "",
      export_options: {
        method: "ad-hoc", # 这可以不指定
        silent: true,  # 隐藏没有必要的信息
        clean: true,  # 在构建前先clean
        output_name: outputName,  # 输出的ipa名称
        thinning: "<none>",
        skip_fetch_profiles:true,
        slack_url:"https://www.baidu.com"
      }
      )

    uploadToFirim
    sh("mv #{outputDir}/.ipa #{outputDir}/#{buildScheme}_#{Time.now.strftime("%Y%m%d_%H%M")}.ipa")

  end

  lane :release do |options|
    buildScheme = ""
    outputDir = "~/Documents/ipa/AppStore/#{Time.now.strftime('%y%m%d')}"
    outputName = "#{buildScheme}_#{Time.now.strftime("%Y-%m-%d-%H:%M:%S")}"

    gym(
      scheme: "",
      workspace: ".xcworkspace",
      silent: true,
      clean: true,
      output_directory: outputDir,
      output_name: outputName,
      configuration: "Release"
      ) # 编译打包 ipa 文件

    deliver(
      force: true,
      skip_screenshots:true,
      skip_metadata: true
      )    # 不上传截屏文件和元数据。
  end

  after_all do |lane|
    desc '成功'
  end

  error do |lane, exception|

  end
end

#自增build号
def updateProjectBuildNumber
  currentTime = Time.new.strftime("%Y%m%d")
  build = get_build_number()
  puts(" build: #{build}  currentTime:#{currentTime}")
  if build.include?"#{currentTime}"
    # => 为当天版本 计算迭代版本号
    puts("为当天版本 计算迭代版本号")
    lastStr = build[build.length-2..build.length-1]
    lastNum = lastStr.to_i
    lastNum = lastNum + 1
    lastStr = lastNum.to_s
    if lastNum < 10
      lastStr = lastStr.insert(0,"0")
    end
    build = "#{currentTime}#{lastStr}"
  else
    # => 非当天版本 build 号重置
    puts("非当天版本 build 号重置")
    build = "#{currentTime}01"
  end
  puts("*************| 更新build #{build} |*************")
  # => 更改项目 build 号
  increment_build_number(
  build_number: "#{build}"
  )
end

# 上传ipa到fir.im服务器，在fir.im获取firim_api_token
def uploadToFirim
  firim(firim_api_token: "")
end
