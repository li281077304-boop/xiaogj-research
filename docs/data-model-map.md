# `wtwo` frontend data model map

- Source: `chenzhengduan/blog`, `wtwo/` frontend only
- Commit: `5561802e8fc2ed699f30abc59e5e942269660db2`
- Analysis date: 2026-10-07
- Method: static inspection only; no execution or network calls

## Scope and counting

`wtwo/src/types/model` contains exactly **35 exported declarations**: 29 interfaces, three string-union aliases, and three enums. Every declaration and declared field/value is mapped below. “关联” is a frontend semantic link, not a claimed database foreign key. “未发现导出模型” means only that this commit has no matching exported declaration under `src/types/model`; it does not imply backend absence.

## Course draft models (9 declarations)

Source: `wtwo/src/types/model/course-draft-req-form-model.ts`.

### `Request`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `AssistantTeacherID` | 助教 ID 列表载体 | `string \| null`，可选 | Employee/助教 | 9-14 |
| `CampusID` | 校区 ID | `null \| string`，可选 | Campus | 15-19 |
| `ClassID` | 班级 ID | `null \| string`，可选 | Class | 20-24 |
| `ClassRoomID` | 教室 ID | `null \| string`，可选 | Classroom | 25-29 |
| `CourseMethod` | 排课方式 | `WTwoCourseModelEnumsCreateCourseTypeEnum`，可选 | 排课方式枚举 | 30 |
| `CourseType` | 上课方式 | `WTwoBusCommonModelDBEnumsCourseTypeEnum`，可选 | 上课方式枚举 | 31 |
| `Date` | 排课日期 | `Date \| null`，可选 | 时间值 | 32-36 |
| `Describe` | 外部备注 | `null \| string`，可选 | 无 | 37-41 |
| `EndTime` | 结束时间 | `Date \| null`，可选 | 时间值 | 42-46 |
| `ID` | 草稿 ID | `null \| string`，可选 | CourseDraft | 47-50 |
| `InternalRemark` | 内部备注 | `null \| string`，可选 | 无 | 51-55 |
| `IsSubscribeCourse` | 是否开放预约 | `number \| null`，可选 | 预约状态 | 56-60 |
| `MainTeacherIDList` | 任课老师 ID 列表 | `string[] \| null`，可选 | Employee/Teacher | 61-65 |
| `ShiftID` | 课程 ID | `null \| string`，可选 | Shift/Course | 66-70 |
| `StartTime` | 开始时间 | `Date \| null`，可选 | 时间值 | 71-75 |
| `StudentUserID` | 学员 ID | `null \| string`，可选 | Student/User | 76-80 |
| `SubjectID` | 科目 ID | `null \| string`，可选 | Subject | 81-85 |

### String-union declarations

| 声明/值 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `WTwoCourseModelEnumsCreateCourseTypeEnum` | 排课方式集合 | string union | `CourseMethod` | 88-91 |
| `按班级排课` | 按班级排课 | string literal | Class | 91 |
| `按学员排课` | 按学员排课 | string literal | Student | 91 |
| `预约课` | 预约课 | string literal | SubscribeCourse | 91 |
| `日程` | 日程 | string literal | Schedule | 91 |
| `WTwoBusCommonModelDBEnumsCourseTypeEnum` | 上课方式集合 | string union | `CourseType` | 93-96 |
| `其他` / `线下课` / `在线课` | 其他 / 线下 / 在线 | string literals | 上课方式 | 96 |
| `WTwoBusCommonModelPublicEnumsYesOrNoEnum` | 通用是否枚举 | string union | `IsSubscribeCourse` | 256-259 |
| `No` / `Yes` | 否 / 是 | string literals | 状态 | 259 |

### `Response`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `Data` | 批量保存结果 | `WTwoCourseModelResponseBatchSaveCourseDraftViewModel`，可选 | 批量草稿结果 | 104-105 |
| `ErrorCode` | 错误码 | `number`，可选 | 响应信封 | 106 |
| `ErrorMsg` | 错误消息 | `null \| string`，可选 | 响应信封 | 107 |
| `IsSuccess` | 是否成功 | `boolean`，可选 | 响应信封 | 108 |

### `WTwoCourseModelResponseBatchSaveCourseDraftViewModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `SavedDraftList` | 保存成功草稿 | `WTwoCourseModelResponseCourseDraftViewModel[] \| null`，可选 | CourseDraft | 114-118 |
| `SaveErrorList` | 保存错误 | `WTwoCourseModelResponseBatchSaveCourseDraftViewModelCourseDraftSaveError[] \| null`，可选 | DraftSaveError | 119-122 |

### `WTwoCourseModelResponseBatchSaveCourseDraftViewModelCourseDraftSaveError`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `DraftId` | 草稿 ID | `null \| string`，可选 | CourseDraft | 128-132 |
| `ErrorFieldList` | 错误字段名 | `string[] \| null`，可选 | `Request` 字段 | 133-136 |
| `ErrorMessage` | 错误原因 | `null \| string`，可选 | 无 | 137-140 |

### `WTwoCourseModelResponseCourseDraftViewModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `AssistantTeacherList` | 助教列表 | `WTwoBusCommonModelCommonCommonDataModelIDNameModel[] \| null`，可选 | Employee/助教 | 146-150 |
| `CampusID` | 校区 ID | `string`，可选 | Campus | 151-154 |
| `CampusName` | 校区名称 | `null \| string`，可选 | Campus | 155-158 |
| `ClassID` | 班级 ID | `string`，可选 | Class | 159-162 |
| `ClassName` | 班级名称 | `null \| string`，可选 | Class | 163-166 |
| `ClassRoomID` | 教室 ID | `string`，可选 | Classroom | 167-170 |
| `ClassRoomName` | 教室名称 | `null \| string`，可选 | Classroom | 171-174 |
| `CourseMethod` | 排课方式 | `WTwoCourseModelEnumsCreateCourseTypeEnum`，可选 | 排课方式 | 175 |
| `CourseType` | 上课方式 | `WTwoBusCommonModelDBEnumsCourseTypeEnum`，可选 | 上课方式 | 176 |
| `CourseTypeName` | 排课类型名称 | `null \| string`，可选 | 排课方式 | 177-180 |
| `CreateTime` | 创建时间 | `Date`，可选 | 时间值 | 181-184 |
| `Date` | 日期 | `Date \| null`，可选 | 时间值 | 185-188 |
| `Describe` | 外部备注 | `null \| string`，可选 | 无 | 189-192 |
| `EndTime` | 结束时间 | `Date \| null`，可选 | 时间值 | 193-196 |
| `ID` | 草稿 ID | `string`，可选 | CourseDraft | 197-200 |
| `InternalRemark` | 内部备注 | `null \| string`，可选 | 无 | 201-204 |
| `IsSubscribeCourse` | 是否开放预约 | `WTwoBusCommonModelPublicEnumsYesOrNoEnum`，可选 | 预约状态 | 205 |
| `IsSubscribeCourseName` | 预约状态名称 | `null \| string`，可选 | 预约状态 | 206-209 |
| `MainTeacherList` | 任课老师列表 | `WTwoBusCommonModelCommonCommonDataModelIDNameModel[] \| null`，可选 | Employee/Teacher | 210-213 |
| `ShiftID` | 课程 ID | `string`，可选 | Shift/Course | 214-217 |
| `ShiftName` | 课程名称 | `null \| string`，可选 | Shift/Course | 218-221 |
| `StartTime` | 开始时间 | `Date \| null`，可选 | 时间值 | 222-225 |
| `StudentName` | 学员姓名 | `null \| string`，可选 | Student | 226-229 |
| `StudentUserID` | 学员 ID | `string`，可选 | Student/User | 230-233 |
| `SubjectID` | 科目 ID | `string`，可选 | Subject | 234-237 |
| `SubjectName` | 科目名称 | `null \| string`，可选 | Subject | 238-241 |
| `UpdateTime` | 更新时间 | `Date`，可选 | 时间值 | 242-245 |

### `WTwoBusCommonModelCommonCommonDataModelIDNameModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ID` | 通用实体 ID | `string`，可选 | ID/名称实体 | 251-252 |
| `Name` | 通用实体名称 | `null \| string`，可选 | ID/名称实体 | 253 |

## Table-course and conflict models (13 declarations)

Source: `wtwo/src/types/model/table-course-class.ts`.

### `TableCourseClass`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `AssistantTeacherID` | 助教 ID | `string` | Employee/助教 | 1-5 |
| `CampusID` | 校区 ID | `string` | Campus | 6-9 |
| `ClassID` | 班级 ID | `string` | Class | 10-13 |
| `ClassRoomID` | 教室 ID（源码注释误写“教师ID”） | `string` | Classroom | 14-17 |
| `CourseMethod` | 排课方式 | `number` | 排课方式 | 18-21 |
| `CourseType` | 上课方式 | `number` | 上课方式 | 22-25 |
| `Date` | 日期 | `string` | 时间值 | 26-29 |
| `Describe` | 外部备注 | `string` | 无 | 30-33 |
| `EndTime` | 结束时间 | `string` | 时间值 | 34-37 |
| `ID` | 草稿/行 ID | `string` | CourseDraft | 38 |
| `InternalRemark` | 内部备注 | `string` | 无 | 39-42 |
| `IsSubscribeCourse` | 是否开放预约 | `number` | 预约状态 | 43-46 |
| `MainTeacherIDList` | 任课老师 ID | `string[]` | Employee/Teacher | 47-50 |
| `ShiftID` | 课程 ID | `string` | Shift/Course | 51-54 |
| `StartTime` | 开始时间 | `string` | 时间值 | 55-58 |
| `StudentUserID` | 学员 ID | `string` | Student/User | 59-62 |
| `SubjectID` | 科目 ID | `string` | Subject | 63-66 |
| `[property: string]` | 动态扩展字段 | `any` | 未约束 | 67 |

### `PreCheckResultData`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `DraftId` | 草稿 ID | `string` | CourseDraft | 71-72 |
| `ErrorFieldList` | 非法字段名 | `string[]` | 草稿字段 | 73 |
| `CheckFieldList` | 限制检查 | `CheckField[]` | CheckField | 74 |
| `ConflictFieldList` | 冲突汇总 | `ConflictField` | ConflictField | 75 |

### `CheckField`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `FieldNameList` | 涉及字段名 | `string[]` | 草稿字段 | 79-80 |
| `ErrorMessage` | 限制错误消息 | `string` | 无 | 81 |
| `CheckerConfigList` | 检查器配置 | `string[]` | 业务规则配置 | 82 |

### `ConflictField`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `FieldNameList` | 冲突字段名 | `string[]` | 草稿字段 | 86-87 |
| `ErrorMessage` | 冲突消息 | `string` | 无 | 88 |
| `ConflictingCourseList` | 冲突课程 | `ConflictingCourse[]` | Course | 89 |
| `ConflictingDraftList` | 冲突草稿 | `any[]` | CourseDraft，结构未约束 | 90 |
| `ConflictingScheduleList` | 冲突日程 | `any[]` | Schedule，结构未约束 | 91 |

### `ConflictingCourse`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ConflictingID` | 冲突对象 ID | `string` | Course/Draft/Schedule | 95-96 |
| `CourseMethod` | 排课方式 | `number` | 排课方式 | 97 |
| `CampusID` | 校区 ID | `string` | Campus | 98 |
| `CampusName` | 校区名称 | `string` | Campus | 99 |
| `ClassID` | 班级 ID | `string` | Class | 100 |
| `ClassName` | 班级名称 | `string` | Class | 101 |
| `ShiftID` | 课程 ID | `string` | Shift/Course | 102 |
| `ShiftName` | 课程名称 | `string` | Shift/Course | 103 |
| `SubjectID` | 科目 ID | `string` | Subject | 104 |
| `SubjectName` | 科目名称 | `string` | Subject | 105 |
| `ClassRoomID` | 教室 ID | `string` | Classroom | 106 |
| `ClassRoomName` | 教室名称 | `string` | Classroom | 107 |
| `StartTime` | 开始时间 | `string` | 时间值 | 108 |
| `EndTime` | 结束时间 | `string` | 时间值 | 109 |

### `CourseDraftPublishDraft_ReqFormModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `DraftIDList` | 草稿 ID 列表 | `string[]` | CourseDraft | 113-114 |
| `CheckConflict` | 是否检查冲突，0 否、1 是 | `number` | 冲突检查 | 115 |

### `CourseDraftPublishDraft_ResFormModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `Data` | 草稿冲突/错误结果 | `WTwoCourseModelDTOCheckConflictCourseDraftSaveErrorDTO[] \| null`，可选 | DraftSaveError | 118-119 |
| `ErrorCode` | 错误码 | `number`，可选 | 响应信封 | 120 |
| `ErrorMsg` | 错误消息 | `null \| string`，可选 | 响应信封 | 121 |
| `IsSuccess` | 是否成功 | `boolean`，可选 | 响应信封 | 122 |

### `WTwoCourseModelDTOCheckConflictCourseDraftSaveErrorDTO`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `CheckFieldList` | 限制字段 | `WTwoCourseModelDTOCheckConflictCheckFieldModel[] \| null`，可选 | CheckField DTO | 128-132 |
| `ConflictFieldList` | 冲突汇总 | `WTwoCourseModelDTOCheckConflictConflictFieldModel`，可选 | ConflictField DTO | 133 |
| `DraftId` | 草稿 ID | `null \| string`，可选 | CourseDraft | 134-137 |
| `ErrorFieldList` | 非法字段 | `WTwoCourseModelDTOCheckConflictErrorFieldModel[] \| null`，可选 | ErrorField DTO | 138-141 |

### `WTwoCourseModelDTOCheckConflictCheckFieldModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `CheckerConfigList` | 检查器配置 | `string[] \| null`，可选 | 业务规则配置 | 147-148 |
| `ErrorMessage` | 限制错误消息 | `null \| string`，可选 | 无 | 149 |
| `FieldNameList` | 涉及字段名 | `string[] \| null`，可选 | 草稿字段 | 150 |

### `WTwoCourseModelDTOCheckConflictConflictFieldModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ConflictingCourseList` | 冲突课程 | `WTwoCourseModelDTOCheckConflictConflictingCourseItemDTO[] \| null`，可选 | Course | 156-160 |
| `ConflictingDraftList` | 冲突草稿 | 同上，可选 | CourseDraft | 161-164 |
| `ConflictingScheduleList` | 冲突日程 | 同上，可选 | Schedule | 165-168 |
| `ErrorMessage` | 冲突消息 | `null \| string`，可选 | 无 | 169 |
| `FieldNameList` | 冲突字段名 | `string[] \| null`，可选 | 草稿字段 | 170 |

### `WTwoCourseModelDTOCheckConflictConflictingCourseItemDTO`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `AssistantTeacherList` | 助教列表 | `WTwoBusCommonModelCommonCommonDataModelIDNameStringModel[] \| null`，可选 | Employee/助教 | 176-180 |
| `CampusID` | 校区 ID | `null \| string`，可选 | Campus | 181-184 |
| `CampusName` | 校区名称 | `null \| string`，可选 | Campus | 185-188 |
| `ClassID` | 班级 ID | `null \| string`，可选 | Class | 189-192 |
| `ClassName` | 班级名称 | `null \| string`，可选 | Class | 193-196 |
| `ClassRoomID` | 教室 ID | `null \| string`，可选 | Classroom | 197-200 |
| `ClassRoomName` | 教室名称 | `null \| string`，可选 | Classroom | 201-204 |
| `ConflictingID` | 冲突对象 ID | `string`，可选 | Course/Draft/Schedule | 205-208 |
| `ConflictingName` | 冲突对象名称 | `null \| string`，可选 | Course/Draft/Schedule | 209 |
| `CourseMethod` | 冲突对象类型/排课方式 | `number`，可选 | 排课方式 | 210-213 |
| `EndTime` | 结束时间 | `null \| string`，可选 | 时间值 | 214 |
| `MainTeacherList` | 任课老师 | `WTwoBusCommonModelCommonCommonDataModelIDNameStringModel[] \| null`，可选 | Employee/Teacher | 215-218 |
| `ShiftID` | 课程 ID | `null \| string`，可选 | Shift/Course | 219-222 |
| `ShiftName` | 课程名称 | `null \| string`，可选 | Shift/Course | 223-226 |
| `StartTime` | 开始时间 | `null \| string`，可选 | 时间值 | 227 |
| `StudentName` | 学员姓名 | `null \| string`，可选 | Student | 228-231 |
| `StudentUserID` | 学员 ID | `null \| string`，可选 | Student/User | 232-235 |
| `SubjectID` | 科目 ID | `null \| string`，可选 | Subject | 236-239 |
| `SubjectName` | 科目名称 | `null \| string`，可选 | Subject | 240-243 |

### `WTwoBusCommonModelCommonCommonDataModelIDNameStringModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ID` | 通用实体 ID | `null \| string`，可选 | ID/名称实体 | 249-250 |
| `Name` | 通用实体名称 | `null \| string`，可选 | ID/名称实体 | 251 |

### `WTwoCourseModelDTOCheckConflictErrorFieldModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ErrorMessage` | 非法字段错误消息 | `null \| string`，可选 | 无 | 257-258 |
| `FieldName` | 非法字段名 | `null \| string`，可选 | 草稿字段 | 259 |

## Timetable preference models (13 declarations)

Source: `wtwo/src/types/model/timetable-preference.ts`.

### Enum declarations and values

| 声明/值 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `CourseTimetableTypeEnum` | 课表对象类型 | enum | 偏好设置 | 1-8 |
| `TeacherTimetable` | 老师课表 | `'TeacherTimetable'` | Employee/Teacher | 3 |
| `StudentTimetable` | 学员课表 | `'StudentTimetable'` | Student | 4 |
| `ClassTimetable` | 班级课表 | `'ClassTimetable'` | Class | 5 |
| `ClassroomTimetable` | 教室课表 | `'ClassroomTimetable'` | Classroom | 6 |
| `TimeTimetable` | 时间课表 | `'TimeTimetable'` | 时间维度 | 7 |
| `CourseTimetableFieldTypeEnum` | 卡片字段类型 | enum | FieldSetting | 10-14 |
| `Main` | 主字段 | `'Main'` | FieldSetting | 12 |
| `Tag` | 标签字段 | `'Tag'` | FieldSetting | 13 |
| `CourseTimetableColorSettingTypeEnum` | 着色维度 | enum | ColorSetting | 16-24 |
| `ByCourseFinished` | 按结课状态 | `'ByCourseFinished'` | Course | 18 |
| `ByTeachingMethod` | 按授课方式 | `'ByTeachingMethod'` | CourseType | 19 |
| `ByTeacher` | 按老师 | `'ByTeacher'` | Employee/Teacher | 20 |
| `ByCourse` | 按课程 | `'ByCourse'` | Shift/Course | 21 |
| `ByOpeningStatus` | 按开班状态 | `'ByOpeningStatus'` | Class | 22 |
| `ByClassroom` | 按教室 | `'ByClassroom'` | Classroom | 23 |

### `CourseTimetableFieldSetting`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `FieldName` | 字段键 | `string \| null` | 课表卡片字段 | 27-28 |
| `DisplayName` | 显示名 | `string \| null` | 课表卡片字段 | 29 |
| `IsEnabled` | 是否启用 | `boolean` | 无 | 30 |
| `IsDefault` | 是否默认 | `boolean` | 无 | 31 |
| `SortOrder` | 排序序号 | `number` | 无 | 32 |
| `FieldType` | 主字段/标签类型 | `CourseTimetableFieldTypeEnum` | 字段类型枚举 | 33 |

### `CourseTimetableColorDetail`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `DisplayName` | 颜色项显示名 | `string \| null` | 着色分组值 | 37-38 |
| `EnumValue` | 枚举值 | `number \| null` | 着色分组值 | 39 |
| `ColorValue` | 颜色值 | `string \| null` | 无 | 40 |

### `CourseTimetableColorSetting`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `SettingType` | 着色维度 | `CourseTimetableColorSettingTypeEnum` | 颜色枚举 | 44-45 |
| `IsEnabled` | 是否启用 | `boolean` | 无 | 46 |
| `ColorDetails` | 颜色明细 | `CourseTimetableColorDetail[] \| null`，可选 | ColorDetail | 47 |

### `CourseTimetableTagColorSetting`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `TagFieldName` | 标签字段键 | `string \| null` | 课表标签字段 | 51-52 |
| `DisplayName` | 标签显示名 | `string \| null` | 课表标签字段 | 53 |
| `ColorValue` | 标签颜色 | `string \| null` | 无 | 54 |

### `SaveTimetablePreference_ReqFormModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `TimetableType` | 课表对象类型 | `CourseTimetableTypeEnum` | 课表类型 | 58-59 |
| `CardShowInformationSettings` | 卡片显示字段 | `CourseTimetableFieldSetting[] \| null` | FieldSetting | 60 |

### `TimetablePreference_ViewModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ID` | 偏好记录 ID | `string` | TimetablePreference | 64-65 |
| `UserID` | 用户 ID | `string` | User | 66 |
| `TimetableType` | 课表对象类型 | `CourseTimetableTypeEnum` | 课表类型 | 67 |
| `TimetableTypeName` | 课表类型名称 | `string \| null` | 课表类型 | 68 |
| `CardShowInformationSettings` | 卡片字段设置 | `CourseTimetableFieldSetting[] \| null` | FieldSetting | 69 |
| `IsEnabled` | 是否启用 | `boolean` | 无 | 70 |
| `CreateTime` | 创建时间 | `string` | 时间值 | 71 |
| `UpdateTime` | 更新时间 | `string` | 时间值 | 72 |
| `TimeViewStart` | 时间视图起点 | `string` | 时间范围 | 73 |
| `TimeViewEnd` | 时间视图终点 | `string` | 时间范围 | 74 |
| `RowHeight` | 行高 | `number`，可选 | UI 偏好 | 75 |

### `SaveTimetableColorPreference_ReqFormModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `TimetableType` | 课表对象类型 | `CourseTimetableTypeEnum` | 课表类型 | 79-80 |
| `ColorSettings` | 主颜色设置 | `CourseTimetableColorSetting[] \| null` | ColorSetting | 81 |
| `TagColorSettings` | 标签颜色设置 | `CourseTimetableTagColorSetting[] \| null` | TagColorSetting | 82 |
| `IsEnabled` | 是否启用 | `boolean` | 无 | 83 |

### `TimetableColorPreference_ViewModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `ID` | 颜色偏好 ID | `string` | TimetableColorPreference | 87-88 |
| `UserID` | 用户 ID | `string` | User | 89 |
| `TimetableType` | 课表对象类型 | `CourseTimetableTypeEnum` | 课表类型 | 90 |
| `TimetableTypeName` | 课表类型名称 | `string \| null` | 课表类型 | 91 |
| `ColorSettings` | 主颜色设置 | `CourseTimetableColorSetting[] \| null` | ColorSetting | 92 |
| `TagColorSettings` | 标签颜色设置 | `CourseTimetableTagColorSetting[] \| null` | TagColorSetting | 93 |
| `IsEnabled` | 是否启用 | `boolean` | 无 | 94 |
| `CreateTime` | 创建时间 | `string` | 时间值 | 95 |
| `UpdateTime` | 更新时间 | `string` | 时间值 | 96 |

### `SaveTimetableTimeRangePreference_ReqFormModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `TimeViewStart` | 时间视图起点 | `string` | 时间范围 | 100-101 |
| `TimeViewEnd` | 时间视图终点 | `string` | 时间范围 | 102 |

### `SaveTimetableRowHeightPreference_ReqFormModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 行 |
|---|---|---|---|---|
| `RowHeight` | 课表行高 | `number` | UI 偏好 | 105-106 |
| `TimetableType` | 课表对象类型 | `CourseTimetableTypeEnum` | 课表类型 | 107 |
| `ApplyToAllTypes` | 应用到全部课表类型 | `boolean` | 课表类型集合 | 108 |

## Shared API declarations outside `types/model` (5)

| 声明.字段 | 中文含义 | TS 类型 | 关联 | 证据 |
|---|---|---|---|---|
| `IOSSFile.url` | 文件 URL | `string` | OSS 文件 | `wtwo/src/types/base.d.ts:2-3` |
| `.name` | 文件名 | `string`，可选 | OSS 文件 | `:4` |
| `.width` / `.height` | 媒体宽 / 高 | `string`，可选 | 媒体元数据 | `:5-6` |
| `.ext` | 扩展名 | `string` | 文件类型 | `:7` |
| `.fileId` / `.fileKey` | 文件 ID / 存储键 | `string`，可选 | OSS 文件 | `:8-9` |
| `.fileSize` | 文件大小 | `string`，可选 | 文件元数据 | `:10` |
| `.type` | 媒体大类 | `'image' \| 'file' \| 'video' \| 'audio'` | 文件类型 | `:11` |
| `.mimeType` | MIME 类型 | `string`，可选 | 文件类型 | `:12` |
| `ISTSParams.fileType` | STS 文件类型 | `1 \| 2 \| 3` | 图片/文档/音视频 | `:15-16` |
| `.businessType` | 上传业务类型 | `string`，可选 | 上传业务 | `:17` |
| `.terminal` | 终端类型 | `0 \| 1`，可选 | 老师端/学员端 | `:18` |
| `IFieldsParams.status` | 字段状态 | `0 \| 1` | 自定义字段 | `wtwo/src/types/fields.d.ts:1-2` |
| `.type` | 字段类型 | `number` | 自定义字段 | `:3` |
| `.table` | 所属表标识 | `string` | 业务对象标识 | `:4` |
| `IFieldsModel.Fields` | 字段键 | `string` | 自定义字段 | `:7-8` |
| `.FieldType` | 字段类型 | `number` | 自定义字段 | `:9` |
| `.ID` / `.Name` | 字段 ID / 名称 | `string` / `string` | 自定义字段 | `:10-11` |
| `.Required` | 是否必填 | `boolean` | 自定义字段 | `:12` |
| `.SelectItem` | 选项数据 | `string` | 自定义字段选项 | `:13` |
| `.Status` | 状态 | `number` | 自定义字段 | `:14` |
| `IDictFieldsModel.ID` / `.Name` / `.Value` | 字典 ID / 名称 / 值 | `string` | Dictionary | `:17-20` |
| `.Describe` | 字典描述 | `string` | Dictionary | `:21` |
| `.Status` | 状态 | `number` | Dictionary | `:22` |
| `.IsSysDefine` | 是否系统定义 | `0 \| 1` | Dictionary | `:23` |

## Requested organization entities

**Exported-model status:** `Company`, `Campus`, `Area`, `Department`, `Employee`, and `User` are all **absent as exported declarations under `wtwo/src/types/model`** in this commit. The following are frontend-local shapes. This does not imply that corresponding backend entities or models are absent.

### Company

No standalone Company interface was found. `useUser().user` has `CompanyID: string` and `CompanyName: string` (`wtwo/src/store/index.ts:41-48`). The employee picker projects these into a company root node with `ID`, `Name`, `PID: ''`, `Type: 1`, `IsCampus: 0`, `Children: []`, `isCompany: true`, and `nodeId`; it uses the component-local `IDepartment` shape rather than a Company model (`wtwo/src/components/popup/chooseEmpAsync.vue:171-180,295-311`).

### Campus — store-local `ICampusModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 证据 |
|---|---|---|---|---|
| `ID` | 校区 ID | `string` | Campus | `wtwo/src/store/organization.ts:37-38` |
| `Name` | 校区名称 | `string` | Campus | `:39` |
| `Pinyin` | 名称拼音 | `string` | Campus | `:40` |
| `IsEnable` | 是否启用 | `number` | 校区状态 | `:41` |
| `AreaID` | 所属区域 ID | `string`，可选 | Area | `:42` |

The organization store caches `_campusList: ICampusModel[]` from `queryAllCampus` (`organization.ts:67-74,115-127,167-170`). `useUserCampuses.userCampuses` is only `any[]` (`wtwo/src/store/index.ts:81-92`).

### Area — store-local `IAreaModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 证据 |
|---|---|---|---|---|
| `ID` | 区域 ID | `string` | Area | `wtwo/src/store/organization.ts:28-29` |
| `Name` | 区域名称 | `string` | Area | `:30` |
| `CampusCount` | 校区数 | `number` | Campus 聚合 | `:31` |
| `CreateTime` | 创建时间 | `string` | 时间值 | `:32` |
| `Describe` | 区域描述 | `string` | Area | `:33` |
| `ParentId` | 父区域 ID | `string` | Area 自关联 | `:34` |

The store caches `_areaList: IAreaModel[]` from `getAreaList` (`organization.ts:67-74,84-90,143-149`).

### Department — store-local `IDepartModel`

| 字段 | 中文含义 | TS 类型 | 关联 | 证据 |
|---|---|---|---|---|
| `Address` | 地址 | `string` | Department/Campus | `wtwo/src/store/organization.ts:4-5` |
| `AreaId` | 区域 ID | `string` | Area | `:6` |
| `AreaName` | 区域名称 | `string` | Area | `:7` |
| `CategoryName` | 分类名称 | `string` | Department 分类 | `:8` |
| `ClassRoomCount` | 教室数 | `number` | Classroom 聚合 | `:9` |
| `EmployeeCount` | 员工数 | `number` | Employee 聚合 | `:10` |
| `ID` | 部门 ID | `string` | Department | `:11` |
| `IsBindingDingTalk` | 是否绑定钉钉 | literal `0` | 集成状态 | `:12` |
| `IsCampus` | 是否校区 | `0 \| 1` | Campus | `:13` |
| `IsEnable` | 是否启用 | `number` | 部门状态 | `:14` |
| `Leader` | 负责人 | `string` | Employee | `:15` |
| `LeaderList` | 负责人列表载体 | `string` | Employee | `:16` |
| `LevelName` | 层级名称 | `string` | 部门层级 | `:17` |
| `LevelString` | 层级路径 | `string` | 部门层级 | `:18` |
| `Name` | 部门名称 | `string` | Department | `:19` |
| `ShiftCount` | 课程数 | literal `0` | Shift/Course 聚合 | `:20` |
| `Tel` | 联系电话 | `string` | Department | `:21` |
| `Type` | 类型值 | `string` | 部门类型 | `:22` |
| `TypeName` | 类型名称 | `string` | 部门类型 | `:23` |
| `Visiable` | 可见状态 | `number` | 权限/状态 | `:24` |
| `parentId` | 父部门 ID | `string` | Department 自关联 | `:25` |

The same store defines `IDepartmentTreeNode`: `ID`, `Name`, `PID`, `value`, and `label` are `string`; `IsCampus` and `Type` are `number`; `Children` is `IDepartmentTreeNode[]` (`organization.ts:45-54`). It builds this tree from department IDs and parent IDs (`:173-218`).

### Employee — component-local `IEmployee`

The organization store's `_departEmps`, `_departEmpUser`, and `_operateCampusEmps` are `any`, so no store-level Employee contract exists (`wtwo/src/store/organization.ts:67-74,91-113,151-165`). The employee picker declares:

| 字段 | 中文含义 | TS 类型 | 关联 | 证据 |
|---|---|---|---|---|
| `ID` | 员工 ID | `string` | Employee | `wtwo/src/components/popup/chooseEmpAsync.vue:181-182` |
| `Name` | 员工姓名 | `string` | Employee | `:183` |
| `DepartID` | 部门 ID | `string` | Department | `:184` |
| `DepartName` | 部门名称 | `string` | Department | `:185` |
| `GradeID` / `GradeName` | 年级 ID / 名称 | `string` | Grade | `:186-187` |
| `GradeList` | 年级列表 | `IDictFields[]` | Grade dictionary | `:188` |
| `SubjectID` / `SubjectName` | 科目 ID / 名称 | `string` | Subject | `:189-190` |
| `SubjectList` | 科目列表 | `IDictFields[]` | Subject dictionary | `:191` |
| `PositionID` / `PositionName` | 职位 ID / 名称 | `string` | Position | `:192-193` |
| `Serial` | 员工编号/序号 | `string` | Employee | `:194` |
| `leaf` | 是否叶节点 | `boolean` | 组织树 | `:196` |
| `isEmp` | 是否员工节点 | `boolean` | 组织树 | `:197` |
| `nodeId` | 树节点 ID | `string` | 组织树 | `:198` |

### User — inline Pinia state

No exported User interface exists. `useUser` defines this inline initial shape, later replaced by `whoami().Data` (`wtwo/src/store/index.ts:41-70`; `wtwo/src/main.ts:99-124`):

| 字段 | 中文含义 | 初始 TS 形状 | 关联 | 证据 |
|---|---|---|---|---|
| `CompanyID` / `CompanyName` | 公司 ID / 名称 | `string` | Company | `wtwo/src/store/index.ts:45-47` |
| `Date` | 服务端日期 | `string` | 时间值 | `:48` |
| `FileServerURL` | 文件服务地址 | `string` | 文件服务 | `:49` |
| `FullName` / `NickName` | 全名 / 昵称 | `string` | User | `:50-51` |
| `ID` | 用户 ID | `string` | User | `:52` |
| `IsAdmin` | 是否管理员 | 初始 `string` | 权限 | `:53` |
| `IsBind` | 是否绑定 | `number`（初值 0） | 账号状态 | `:54` |
| `IsUpdatePwd` | 是否更新密码 | `number`（初值 0） | 账号状态 | `:55` |
| `Photo` | 头像地址 | `string` | User | `:56` |
| `SMSTel` | 短信手机号 | `string` | User | `:57` |
| `Sex` | 性别 | `string` | User | `:58` |
| `SsxStatus` | 状态标志，源码未注释语义 | `boolean` | User 状态 | `:59` |
| `Time` | 服务端时间 | `string` | 时间值 | `:60` |
| `UserName` / `UserNameSuffix` | 用户名 / 后缀 | `string` | User | `:61-62` |
| `CustomerValue` | 客户值，源码未注释语义 | `number`（初值 0） | User/Company | `:63` |
| `TrainingTermNo` | 培训期编号 | `string` | TrainingTerm | `:64` |
| `SID` | 会话/用户 SID | `string` | Session | `:65` |

### Login and current-campus state — inline Pinia shapes

These are session/UI state, not exported domain models:

| 字段 | 中文含义 | 初始 TS 形状 | 关联 | 证据 |
|---|---|---|---|---|
| `loginInfo.WTwo_CompanyID` | 当前公司/租户 ID | `string` | Company | `wtwo/src/store/index.ts:29-35` |
| `loginInfo.WTwo_AuthToken` | 当前认证令牌 | `string` | Session/Auth；仅记录字段名 | `:35` |
| `useUserCampuses.userCampuses` | 当前用户可用校区 | `any[]` | Campus，未约束元素模型 | `:81-85` |
| `useCurrentCampuses.campusList` | 当前校区 ID 列表载体 | `string` | Campus | `:94-98` |
| `useCurrentCampuses.multi` | 是否多校区模式 | `boolean`（初值 `true`） | Campus selection | `:98` |

## Requested entity coverage matrix

This matrix makes the requested entity set explicit. “Absent” always means absent as a dedicated exported model declaration in `src/types/model`, not absent from the product or backend.

| Requested entity | Exported declaration status | Best frontend evidence in this commit |
|---|---|---|
| Company | **Absent** as dedicated exported model | Only `CompanyID`/`CompanyName` in inline user state and a company root projected into component-local `IDepartment`; see Company section above and `store/index.ts:45-47`, `chooseEmpAsync.vue:295-311`. |
| Campus | **Absent** as dedicated exported model | Store-local `ICampusModel` has ID, name, pinyin, enable status, area ID; exported draft/conflict models carry campus ID/name references. See Campus section; `course-draft-req-form-model.ts:151-158`. |
| Area | **Absent** as dedicated exported model | Store-local `IAreaModel`; see Area section. No exported model relation beyond campus's local `AreaID`. |
| Department | **Absent** as dedicated exported model | Store-local `IDepartModel` and `IDepartmentTreeNode`; see Department section. |
| Employee | **Absent** as dedicated exported model | Organization caches are `any`; component-local `IEmployee` provides the only detailed employee shape. See Employee section. |
| User | **Absent** as dedicated exported model | Inline `useUser().user` shape; exported preference models contain only `UserID`. See User section and `timetable-preference.ts:64-76,87-97`. |
| Student | **Absent** as dedicated exported model | Exported models provide only `StudentUserID`/`StudentName` references (`course-draft-req-form-model.ts:226-233`; `table-course-class.ts:228-235`). Component-local `StudentInfo` has ID, name, photo, code, campus name, labels, serial, type and master (`studentDetailPopover.vue:111-121`); attendance UI adds a local `IStudent` (`arrangeInfo.vue:1009-1031`). |
| Class | **Absent** as dedicated exported model | Only `ClassID`/`ClassName` references in draft/conflict models (`course-draft-req-form-model.ts:159-166`; `table-course-class.ts:189-196`). API wrappers otherwise use `any`. |
| Shift | **Absent** as dedicated exported model | Only course/shift ID/name references (`course-draft-req-form-model.ts:214-221`; `table-course-class.ts:219-226`). |
| Subject | **Absent** as dedicated exported model | Only subject ID/name references (`course-draft-req-form-model.ts:234-241`; `table-course-class.ts:236-243`) and dictionary-backed local lists. |
| Teacher | **Absent** as dedicated exported model | Exported models use generic ID/name lists for `MainTeacherList` (`course-draft-req-form-model.ts:210-213`). Local views define `{id,name,avatar?,desc?,campusId?}` (`periodCanlendarView3.vue:292-298`) and `TeacherInfo{Name,NickName,ID}` (`appointTeacherForm.vue:126-142`). |
| AssistantTeacher | **Absent** as dedicated exported model | Only `AssistantTeacherID` and generic ID/name lists in draft/conflict declarations (`course-draft-req-form-model.ts:9-14,146-150`; `table-course-class.ts:176-180`). |
| Classroom | **Absent** as dedicated exported model | Exported models provide only classroom ID/name references (`course-draft-req-form-model.ts:167-174`; `table-course-class.ts:197-204`). A picker has open local type `{ID: string \| number; Name: string; [key:string]: any}` (`components/popup/chooseClassroom.vue:54`). |
| CourseDraft | **Present indirectly**, without a declaration literally named `CourseDraft` | `Request`, `Response`, `WTwoCourseModelResponseCourseDraftViewModel`, `TableCourseClass`, publish/precheck DTOs form the exported draft contract; see the first two model sections. |
| CoursePlan | **Absent** as dedicated exported model | Endpoint traces exist, but request/response payloads are `any`: add/preview/get/edit/reuse operations in `api/arrange.ts:121-128,541-559,727-733,767-773,800-806`. |
| Course | **Absent** as dedicated canonical exported model | Exported conflict models describe course references/items, not a complete course aggregate (`table-course-class.ts:94-110,173-244`). Component-local `CourseData` describes popover display fields and permits arbitrary extensions (`components/CourseDetailPopover.vue:175-193`). |
| Schedule | **Absent** as dedicated exported model | Schedule is represented only as one conflict list variant and by shared conflict items (`table-course-class.ts:165-168,205-243`). `CourseData.type` distinguishes `'course' \| 'schedule'` and adds `Content`/`Remark` (`CourseDetailPopover.vue:175-192`). |
| SubscribeCourse | **Absent** as dedicated exported model | Exported models contain subscribe flags/names (`course-draft-req-form-model.ts:56-60,205-209`). Component-local `ISubscribeStudent` describes预约学员 fields (`arrangeInfo.vue:1041-1050`); API wrappers remain `any` (`api/arrange.ts:213-223,320-323,434-460,493-496`). |
| Exam | **Absent** as dedicated exported model | Exam API wrappers use `any`; routes cover list/create/detail/update/delete and related queries (`api/exam.ts:4-75`). Page-local reactive objects are not a stable exported contract. |
| Score | **Absent** as dedicated exported model | Score request/response wrappers use `any` except upload input `FormData`; batch/detail/analysis/statistics routes are visible (`api/exam.ts:77-203`), but no exported score field model exists. |
| FinancialPeriod | **Absent** as dedicated exported model | `periodList` is `ref<any[]>`; observed UI fields include `ID`, `StartDate`, `EndDate`, `Pass`, `isEditing`, `editDateRange` (`pages/financialManage/financialManage.vue:247-272,294-330,394-448`). API parameters are `any` (`api/financial.ts:3-40`). |
| UserPreference | **Present as specialized exported timetable preference models**, but no generic `UserPreference` declaration | `TimetablePreference_ViewModel`, color and save request models are exported (`types/model/timetable-preference.ts:57-109`). A separate store-local `UserSetting` has `ID?`, `PageKey`, `PageName`, `Type`, `IsPublic`, `UserSettingsDetailList?` (`store/userSettings.ts:4-23`). |

## API binding and model drift

The shared client returns `IResponse<T>` with `ErrorCode`, `Data`, `ErrorMsg`, optional pagination fields, and `handleCode` (`wtwo/common/tool/http/fetch.ts:1-28,83-108`). Confirmed bindings are: custom fields (`wtwo/src/api/comm.ts:3-11`), dictionaries (`comm.ts:13-27`), and all timetable preferences (`wtwo/src/api/arrange.ts:90-95`). Time-range and row-height saves have typed requests but `IResponse<any>` (`arrange.ts:97-111`); publish draft has a typed request but does not attach the declared response type (`arrange.ts:562-568`).

Material drift/ambiguity:

1. `TableCourseClass` uses string dates/times and numeric flags; generated `Request` uses `Date` and string unions (`table-course-class.ts:18-29,34-58`; `course-draft-req-form-model.ts:30-36,42-46,70-75,88-96`).
2. `PreCheckResultData.ErrorFieldList` is `string[]`; the DTO version uses `{FieldName, ErrorMessage}` objects (`table-course-class.ts:71-76,128-142,254-260`).
3. `IResponse<T>` omits `IsSuccess`, while draft-save UI reads it and generated envelopes declare it (`wtwo/common/tool/http/fetch.ts:13-22`; `class-table-course.vue:1003-1016`; `course-draft-req-form-model.ts:104-109`).
4. Most wrappers use `any`; a similarly named frontend declaration is a contract candidate, not proof of deployed server compatibility.
