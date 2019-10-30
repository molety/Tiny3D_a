######## �A�v���b�g �r���h�pRakefile

require 'rake/clean'


### ���������ɍ��킹�ď��������遫����

# jar�t�@�C����
JAR_NAME = "Tiny3D_a.jar"
# Java�����^�C�����C�u�����̃p�X
LIB_CLASSPATH = "C:/PROGRA~1/Java/jdk1.6.0_11/jre/lib/rt.jar"
# ProGuard�̃p�X
PROGUARD_PATH = "C:/WTK2.5.1/bin/proguard.jar"
# 7-Zip�̃p�X
SEVENZIP_PATH = "\"C:/Program Files/7-Zip/7z.exe\""

# �R���p�C���Ώۂ̃\�[�X
source_files = FileList["src/*.java"]
source_files.exclude("**/Test*.java")  # ���O�������t�@�C��������΂�����
# jar�Ɋ܂߂郊�\�[�X
res_files = FileList["res/*"]

# jar�𐶐�����f�B���N�g��
PACKAGE_DIR = "bin"

### ���������ɍ��킹�ď��������遪����


PREPROCESSED_DIR = "preprocessed"
COMPILED_DIR = "compiled"
OBFUSCATED_DIR = "obfuscated"
PACKAGE_TMP_DIR = "pkg_tmp"

new_jar_path = "#{PACKAGE_DIR}/#{JAR_NAME}"

CLEAN.include(PREPROCESSED_DIR, COMPILED_DIR, OBFUSCATED_DIR, PACKAGE_TMP_DIR)
if PACKAGE_DIR != "bin"  # bin�f�B���N�g���͍폜�Ώۂɂ��Ȃ�
  CLOBBER.include(PACKAGE_DIR)
end

directory PREPROCESSED_DIR
directory COMPILED_DIR
directory OBFUSCATED_DIR
directory PACKAGE_TMP_DIR
directory PACKAGE_DIR

desc "�v���v���Z�X"
task :preprocess => [PREPROCESSED_DIR] do
  rm Dir.glob("#{PREPROCESSED_DIR}/*")
  source_files.each do |path|
    source = IO.readlines(path)
    prepro_source = Array.new
    skip = false
### ���������ɍ��킹�ď��������遫����
    # ���̕ӂ�ōD���Ȃ悤�Ƀ\�[�X�����H�ł��܂��B
    source.each do |line|
      # //#ifdef DEBUG �` //#endif �ň͂܂ꂽ�s���R�����g�A�E�g����
      if /^\s*\/\/\s*#ifdef\s+DEBUG/ =~ line
        skip = true
      elsif /^\s*\/\/\s*#endif/ =~ line
        skip = false
      else
        line.gsub!(/^/, "//") if skip
      end
        prepro_source.push(line)
    end
### ���������ɍ��킹�ď��������遪����
    prepro_path = path.gsub(/^src/, PREPROCESSED_DIR)
    File.open(prepro_path, "w") do |file|
      file.write(prepro_source)
    end
  end
end

desc "�R���p�C��"
task :compile => [:preprocess, COMPILED_DIR] do
  rm Dir.glob("#{COMPILED_DIR}/*")
  sh "javac -source 1.4 -target 1.4 -d #{COMPILED_DIR} #{PREPROCESSED_DIR}/*.java 2> err_log.txt"
end

desc "��ǉ��^�œK��"
task :obfuscate => [:compile, OBFUSCATED_DIR] do
  rm Dir.glob("#{OBFUSCATED_DIR}/*")
  sh "java -jar #{PROGUARD_PATH} -injars #{COMPILED_DIR} -outjars #{OBFUSCATED_DIR} -libraryjars #{LIB_CLASSPATH} -keep public class '*' extends java.applet.Applet"
end

desc "jar�쐬"
task :jar => [:obfuscate, PACKAGE_TMP_DIR, PACKAGE_DIR] do
  rm Dir.glob("#{PACKAGE_TMP_DIR}/*")
  rm_f new_jar_path
  cp FileList["#{OBFUSCATED_DIR}/*"], PACKAGE_TMP_DIR, {:preserve => true}
  cp res_files, PACKAGE_TMP_DIR, {:preserve => true}
  cd PACKAGE_TMP_DIR do
    sh "#{SEVENZIP_PATH} a -tzip -mx=9 -mfb=128 ../#{new_jar_path} *"
  end
end

desc "javadoc�쐬"
task :docs => [] do
  sh "javadoc -private -use -classpath #{LIB_CLASSPATH} -d docs src/*.java"
end

task :default => [:jar]
