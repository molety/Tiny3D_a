######## アプレット ビルド用Rakefile

require 'rake/clean'


### ↓↓↓環境に合わせて書き換える↓↓↓

# jarファイル名
JAR_NAME = "Tiny3D_a.jar"
# Javaランタイムライブラリのパス
LIB_CLASSPATH = "C:/PROGRA~1/Java/jdk1.6.0_11/jre/lib/rt.jar"
# ProGuardのパス
PROGUARD_PATH = "C:/WTK2.5.1/bin/proguard.jar"
# 7-Zipのパス
SEVENZIP_PATH = "\"C:/Program Files/7-Zip/7z.exe\""

# コンパイル対象のソース
source_files = FileList["src/*.java"]
source_files.exclude("**/Test*.java")  # 除外したいファイルがあればここへ
# jarに含めるリソース
res_files = FileList["res/*"]

# jarを生成するディレクトリ
PACKAGE_DIR = "bin"

### ↑↑↑環境に合わせて書き換える↑↑↑


PREPROCESSED_DIR = "preprocessed"
COMPILED_DIR = "compiled"
OBFUSCATED_DIR = "obfuscated"
PACKAGE_TMP_DIR = "pkg_tmp"

new_jar_path = "#{PACKAGE_DIR}/#{JAR_NAME}"

CLEAN.include(PREPROCESSED_DIR, COMPILED_DIR, OBFUSCATED_DIR, PACKAGE_TMP_DIR)
if PACKAGE_DIR != "bin"  # binディレクトリは削除対象にしない
  CLOBBER.include(PACKAGE_DIR)
end

directory PREPROCESSED_DIR
directory COMPILED_DIR
directory OBFUSCATED_DIR
directory PACKAGE_TMP_DIR
directory PACKAGE_DIR

desc "プリプロセス"
task :preprocess => [PREPROCESSED_DIR] do
  rm Dir.glob("#{PREPROCESSED_DIR}/*")
  source_files.each do |path|
    source = IO.readlines(path)
    prepro_source = Array.new
    skip = false
### ↓↓↓環境に合わせて書き換える↓↓↓
    # この辺りで好きなようにソースを加工できます。
    source.each do |line|
      # //#ifdef DEBUG ～ //#endif で囲まれた行をコメントアウトする
      if /^\s*\/\/\s*#ifdef\s+DEBUG/ =~ line
        skip = true
      elsif /^\s*\/\/\s*#endif/ =~ line
        skip = false
      else
        line.gsub!(/^/, "//") if skip
      end
        prepro_source.push(line)
    end
### ↑↑↑環境に合わせて書き換える↑↑↑
    prepro_path = path.gsub(/^src/, PREPROCESSED_DIR)
    File.open(prepro_path, "w") do |file|
      file.write(prepro_source)
    end
  end
end

desc "コンパイル"
task :compile => [:preprocess, COMPILED_DIR] do
  rm Dir.glob("#{COMPILED_DIR}/*")
  sh "javac -source 1.4 -target 1.4 -d #{COMPILED_DIR} #{PREPROCESSED_DIR}/*.java 2> err_log.txt"
end

desc "難読化／最適化"
task :obfuscate => [:compile, OBFUSCATED_DIR] do
  rm Dir.glob("#{OBFUSCATED_DIR}/*")
  sh "java -jar #{PROGUARD_PATH} -injars #{COMPILED_DIR} -outjars #{OBFUSCATED_DIR} -libraryjars #{LIB_CLASSPATH} -keep public class '*' extends java.applet.Applet"
end

desc "jar作成"
task :jar => [:obfuscate, PACKAGE_TMP_DIR, PACKAGE_DIR] do
  rm Dir.glob("#{PACKAGE_TMP_DIR}/*")
  rm_f new_jar_path
  cp FileList["#{OBFUSCATED_DIR}/*"], PACKAGE_TMP_DIR, {:preserve => true}
  cp res_files, PACKAGE_TMP_DIR, {:preserve => true}
  cd PACKAGE_TMP_DIR do
    sh "#{SEVENZIP_PATH} a -tzip -mx=9 -mfb=128 ../#{new_jar_path} *"
  end
end

desc "javadoc作成"
task :docs => [] do
  sh "javadoc -private -use -classpath #{LIB_CLASSPATH} -d docs src/*.java"
end

task :default => [:jar]
