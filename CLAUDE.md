# trip-map-builder - 旅行地图技能包
Markdown + optional Google Maps HTML + Leaflet/OpenStreetMap fallback + OpenCLI references

<directory>
assets/ - 单文件地图模板 (2文件: template.html 免费底图 + Google 导航；template-google.html 已配置时优先使用的 Google Maps 底图)
references/ - 调研、规划与地图配置 (4文件: trip-planning.md, dianping-research.md, xhs-research.md, google-maps.md)
</directory>

<config>
README.md - 对外说明技能定位、安装、流程和目录结构
SKILL.md - Agent 技能入口，定义触发条件、共享记忆和三阶段流程
</config>

法则: 行程是参考坐标，不是执行脚本。记忆只存下次仍有用的偏好。餐厅先看当天区域，再看大众点评和小红书。

[PROTOCOL]: 变更时更新此头部，然后检查 CLAUDE.md
